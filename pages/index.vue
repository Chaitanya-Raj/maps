<template>
  <div>
    <div id="map" class="min-w-full min-h-screen"></div>
    <search />
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import Search from "~/components/Search.vue";
export default {
  components: { Search },
  name: "IndexPage",
  data() {
    return {
      map: {},
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
      // this.map = new mapboxgl.Map({
      //   container: "map",
      //   style: "mapbox://styles/mapbox/streets-v11",
      // });
    },
  },
};
</script>
