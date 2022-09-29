<template>
  <div>
    <div id="map" class="font-poppins min-w-full min-h-screen"></div>
    <search
      @search="(items) => addMarkers(items)"
      @fly="(coordinates) => flyToLocation(coordinates)"
    />
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import Search from "~/components/Search.vue";
export default {
  components: { Search },
  name: "IndexPage",
  data() {
    return {
      map: {},
      markers: [],
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
      this.map = new mapboxgl.Map({
        container: "map",
        // style: "mapbox://styles/mapbox/streets-v11?optimize=true",
        style: "mapbox://styles/chaitanyaraj/cl8lvjgqa000m15nt3i9wihwr",
      });

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
    },
    addMarkers(items) {
      // this.markers.forEach((marker) => marker.remove());
      // items.forEach((item) => {
      //   const el = document.createElement("div");
      //   el.className =
      //     "bg-white px-2 py-1 border border-gray-400 flex rounded-lg font-poppins";

      //   const img = document.createElement("img");
      //   img.src = item.photo;
      //   img.className = "h-8 mr-2 rounded-full";

      //   const text = document.createElement("div");
      //   text.className = "flex flex-col";

      //   const name = document.createElement("p");
      //   name.innerText = item.fullName;
      //   name.className = "text-sm";

      //   const company = document.createElement("p");
      //   company.innerText = item.companyName;
      //   company.className = "text-gray-500 text-xs";

      //   text.appendChild(name);
      //   text.appendChild(company);

      //   el.appendChild(img);
      //   el.appendChild(text);

      //   // el.addEventListener(
      //   //   "click",
      //   //   this.flyToLocation([item["location.lng"], item["location.lat"]])
      //   // );

      //   this.markers.push(
      //     new mapboxgl.Marker(el)
      //       .setLngLat([item["location.lng"], item["location.lat"]])
      //       .addTo(this.map)
      //   );
      // });

      this.map.on("load", () => {
        const feat = items.map((item) => {
          // Add an image to use as a custom marker
          this.map.loadImage(item.photo, (error, image) => {
            if (error);
            this.map.addImage(item.objectID, image);
          });
          return {
            type: "Feature",
            geometry: {
              type: "Point",
              coordinates: [item["location.lng"], item["location.lat"]],
            },
            properties: {
              title: item.fullName,
              icon: item.objectID,
            },
          };
        });
        this.map.addSource("users", {
          type: "geojson",
          tolerance: 0,
          data: {
            type: "FeatureCollection",
            features: feat,
          },
        });

        // Add a symbol layer
        this.map.addLayer({
          id: "users",
          type: "symbol",
          source: "users",
          layout: {
            "icon-image": ["get", "icon"],
            // "icon-allow-overlap": true,
            "icon-size": 0.75,
            // get the title name from the source's "title" property
            "text-field": ["get", "title"],
            "text-optional": true,
            // "text-allow-overlap": true,
            "text-font": ["Open Sans Semibold", "Arial Unicode MS Bold"],
            "text-offset": [0, 2],
            "text-anchor": "top",
          },
        });
      });
    },
    //Can be removed to improve performance
    flyToLocation(coordinates) {
      this.map.flyTo({
        center: coordinates,
        duration: 8000,
        zoom: 12.5,
      });
    },
  },
};
</script>
