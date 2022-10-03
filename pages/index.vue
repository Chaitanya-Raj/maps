<template>
  <div>
    <div class="transition-all" :class="selectedUser ? 'brightness-75' : ''">
      <div id="map" class="font-mulish min-w-full min-h-screen"></div>

      <div class="bottom-6 right-2 fixed flex flex-col items-end gap-2">
        <user-count :count="memberCount" />
        <brand-tag />
      </div>
    </div>
    <search
      @add-markers="(items) => addMarkers(items)"
      @fly="(coordinates) => flyToLocation(coordinates)"
      @show-modal="(hit) => toggleModal(hit)"
      @update-markers="(items) => updateMarkers(items)"
    />
    <transition>
      <profile
        v-if="selectedUser"
        :user="selectedUser"
        v-on-clickaway="() => toggleModal(false)"
      />
    </transition>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import Search from "~/components/Search.vue";
import Profile from "~/components/Profile.vue";
import UserCount from "~/components/UserCount.vue";
import BrandTag from "~/components/BrandTag.vue";
export default {
  components: { Search, Profile, UserCount, BrandTag },
  name: "IndexPage",
  data() {
    return {
      map: {},
      markers: [],
      selectedUser: false,
      showClustered: false,
      memberCount: 0,
    };
  },
  mounted() {
    this.createMap();
  },
  computed: {
    mapboxAccessToken() {
      return this.$config.MAPBOX_API_KEY;
    },
  },
  methods: {
    createMap() {
      mapboxgl.accessToken = this.mapboxAccessToken;

      // Initialize the map.
      this.map = new mapboxgl.Map({
        container: "map",
        // style: "mapbox://styles/mapbox/streets-v11?optimize=true",
        // style: "mapbox://styles/chaitanyaraj/cl8lvjgqa000m15nt3i9wihwr?optimize=true",
        style:
          "mapbox://styles/chaitanyaraj/cl8oazq8v002115lgjwjkbixh?optimize=true",
        zoom: 2,
        minZoom: 1.75,
        center: [-9.142685, 38.736946],
        projection: "mercator",
        // projection: "globe",
      });

      // Add zoom and rotation controls to the map.
      this.map.addControl(new mapboxgl.NavigationControl());

      // Add user location control to the map.
      this.map.addControl(
        new mapboxgl.GeolocateControl({
          positionOptions: {
            enableHighAccuracy: true,
          },
          // When active the map will receive updates to the device's location as it changes.
          trackUserLocation: true,
          // Draw an arrow next to the location dot to indicate which direction the device is heading.
          showUserHeading: true,
        })
      );

      this.map.loadImage(
        "https://cdn-icons-png.flaticon.com/128/4390/4390411.png",
        (error, image) => {
          if (error) console.log(error);
          this.map.addImage("custom-marker", image);
        }
      );
    },
    addMarkers(items) {
      this.memberCount = items.length;

      this.map.on("load", () => {
        this.updateMarkers(items);
      });
    },

    updateMarkers(items) {
      if (this.map.getLayer("unclustered-point")) {
        this.map.removeLayer("unclustered-point");
      }
      if (this.map.getSource("users")) {
        this.map.removeSource("users");
      }
      const feat = items.map((item) => {
        return {
          type: "Feature",
          geometry: {
            type: "Point",
            coordinates: [item._geoloc.lng, item._geoloc.lat],
          },
          properties: {
            name: item.fullName,
            gender: item.gender,
            company: item.companyName,
            designation: item.designation,
            city: item.location.city,
            country: item.location.city,
            objectID: item.objectID,
          },
        };
      });
      this.map.addSource("users", {
        type: "geojson",
        data: {
          type: "FeatureCollection",
          features: feat,
        },
        tolerance: 0,
        cluster: this.showClustered,
        clusterMaxZoom: 14, // Max zoom to cluster points on
        clusterRadius: 40, // Radius of each cluster when clustering points (defaults to 50)
      });

      if (this.showClustered) {
        console.log("cluster");
        this.map.addLayer({
          id: "clusters",
          type: "circle",
          source: "users",
          filter: ["has", "point_count"],
          paint: {
            // Use step expressions (https://docs.mapbox.com/mapbox-gl-js/style-spec/#expressions-step)
            // with three steps to implement three types of circles:
            //   * Blue, 20px circles when point count is less than 100
            //   * Yellow, 30px circles when point count is between 100 and 750
            //   * Pink, 40px circles when point count is greater than or equal to 750
            "circle-color": [
              "step",
              ["get", "point_count"],
              "#7CC6FE",
              30,
              "#FCB0B3",
              60,
              "#DBD56E",
            ],
            "circle-radius": [
              "step",
              ["get", "point_count"],
              20,
              100,
              30,
              750,
              40,
            ],
            "circle-opacity": 0.5,
          },
        });

        // Add a symbol layer
        this.map.addLayer({
          id: "cluster-count",
          type: "symbol",
          source: "users",
          filter: ["has", "point_count"],
          layout: {
            "text-field": "{point_count_abbreviated}",
            "text-font": ["DIN Offc Pro Medium", "Arial Unicode MS Bold"],
            "text-size": 12,
          },
        });

        // inspect a cluster on click
        this.map.on("click", "clusters", (e) => {
          const features = this.map.queryRenderedFeatures(e.point, {
            layers: ["clusters"],
          });
          const clusterId = features[0].properties.cluster_id;
          this.map
            .getSource("users")
            .getClusterExpansionZoom(clusterId, (err, zoom) => {
              if (err) return;
              console.log(features[0]);
              this.map.easeTo({
                center: features[0].geometry.coordinates,
                zoom: zoom,
              });
            });
        });

        // Change the cursor to a pointer when the it enters a feature in the 'circle' layer.
        this.map.on("mouseenter", "clusters", () => {
          this.map.getCanvas().style.cursor = "pointer";
        });

        // Change it back to a pointer when it leaves.
        this.map.on("mouseleave", "clusters", () => {
          this.map.getCanvas().style.cursor = "";
        });
      }

      // Add individual points
      this.map.addLayer({
        id: "unclustered-point",
        type: "symbol",
        source: "users",
        filter: ["!", ["has", "point_count"]],
        layout: {
          "icon-image": "custom-marker",
          "icon-allow-overlap": true,
          "icon-size": 0.2,
          "text-field": ["get", "name"],
          "text-optional": true,
          "text-size": 14,
          // "text-allow-overlap": true,
          "text-font": ["Lato Regular", "PT Sans Regular"],
          "text-offset": [0, 1.3],
          "text-anchor": "top",
        },
        paint: {
          "icon-opacity": 0.7,
          "text-color": "#1e354d",
        },
      });

      // Center the map on the coordinates of any clicked circle from the 'users' layer.
      this.map.on("click", "unclustered-point", (e) => {
        this.flyToLocation(e.features[0].geometry.coordinates);
        this.toggleModal(
          items.find(
            (item) => item.objectID === e.features[0].properties.objectID
          )
        );
      });

      // Change the cursor to a pointer when the it enters a feature in the 'circle' layer.
      this.map.on("mouseenter", "unclustered-point", () => {
        this.map.getCanvas().style.cursor = "pointer";
      });

      // Change it back to a pointer when it leaves.
      this.map.on("mouseleave", "unclustered-point", () => {
        this.map.getCanvas().style.cursor = "";
      });
    },

    //Can be removed to improve performance
    flyToLocation(coordinates) {
      this.map.flyTo({
        center: coordinates,
        duration: 8000,
        zoom: 12.5,
        essential: true,
        // pitch: 75,
      });
    },

    toggleModal(f) {
      this.selectedUser = f;
    },
  },
};
</script>

<style>
.v-enter-active,
.v-leave-active {
  transition: all 2s ease;
}

.v-enter,
.v-leave-to {
  opacity: 0;
  transform: translateX(100%);
}

.mapboxgl-ctrl-bottom-right {
  font-size: 10px;
}
</style>
