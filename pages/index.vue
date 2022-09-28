<template>
  <div>
    <div id="map" class="min-w-full min-h-screen"></div>
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
        style: "mapbox://styles/mapbox/streets-v11",
      });
    },
    addMarkers(items) {
      this.markers.forEach((marker) => marker.remove());
      items.forEach((item) => {
        const el = document.createElement("div");
        el.className =
          "bg-white px-2 py-1 border border-gray-400 flex rounded-lg";

        const img = document.createElement("img");
        img.src = item.photo;
        img.className = "h-8 mr-2 rounded-full";

        const text = document.createElement("div");
        text.className = "flex flex-col";

        const name = document.createElement("p");
        name.innerText = item.fullName;
        name.className = "text-sm";

        const company = document.createElement("p");
        company.innerText = item.companyName;
        company.className = "text-gray-500 text-xs";

        text.appendChild(name);
        text.appendChild(company);

        el.appendChild(img);
        el.appendChild(text);

        this.markers.push(
          new mapboxgl.Marker(el)
            .setLngLat([item["location.lng"], item["location.lat"]])
            .addTo(this.map)
        );
      });
    },
    flyToLocation(coordinates) {
      console.log(coordinates);
      this.map.flyTo({
        center: coordinates,
        essential: true,
      });
    },
  },
};
</script>
