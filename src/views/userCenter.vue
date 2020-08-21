<template>
  <div class="map">
    <l-map style="width:100%; height: 100%" :zoom="zoom" :center="currentPosition">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :lat-lng="currentPosition">
        <l-popup>
          <div>currentPosition</div>
        </l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";

export default {
  name: "Map",
  components: { LMap, LTileLayer, LMarker, LPopup },
  data: function () {
    return {
      zoom: 13,
      // will be rendered
      currentPosition: [51.505, -0.09],
      url: "http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    };
  },
  mounted() {
    this.askPermissionForCurrentPosition();
  },
  methods: {
    askPermissionForCurrentPosition() {
      const vm = this;
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((position) => {
          vm.currentPosition = L.latLng(
            position.coords.latitude,
            position.coords.longitude
          );
        });
      } else {
        alert("Geolocation is not supported by this browser.");
      }
      return vm.currentPosition;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.map {
  height: 500px;
  width: 500px;
}
</style>
