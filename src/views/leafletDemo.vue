<template>
  <l-map
    style="width: 100%; height: 620px; z-index: 0"
    :center="center"
    :zoom="zoom"
    ref="map"
    :options="{zoomControl:false}"
  >
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <!-- <l-marker v-for="site in siteMarkers" :key="site.name" :lat-lng="site.location">
      <l-popup :content="site.name" />
    </l-marker>-->
    <l-marker :lat-lng="[22.464, 114.216]">
      <l-popup :content="content"></l-popup>
    </l-marker>
    <l-marker :lat-lng="[22.352, 114.15]" draggable @add="openPopup">
      <l-popup :content="`Drag me around`" :options="{ autoClose: false, closeOnClick: false }"></l-popup>
    </l-marker>
    <l-marker :lat-lng="[22.238, 114.226]">
      <l-popup :content="template"></l-popup>
    </l-marker>
    <l-control-scale position="bottomleft" :metric="true" :imperial="true"></l-control-scale>
    <v-geosearch :options="geosearchOptions" position="topright"></v-geosearch>
    <v-locatecontrol />
    <vue-leaflet-minimap :layer="minimapLayer" :options="options"></vue-leaflet-minimap>
  </l-map>
</template>
 
<script>
import * as L from "leaflet";
import "leaflet/dist/leaflet.css";
import VueLeafletMinimap from "vue-leaflet-minimap";
import "leaflet-minimap/dist/Control.MiniMap.min.css";
import { LMap, LTileLayer, LControlScale, LMarker, LPopup } from "vue2-leaflet";
import { OpenStreetMapProvider } from "leaflet-geosearch";
import "leaflet-geosearch/assets/css/leaflet.css";
import VGeosearch from "vue2-leaflet-geosearch";
import Vue2LeafletLocatecontrol from "vue2-leaflet-locatecontrol/Vue2LeafletLocatecontrol";
import "leaflet-measure";
import "leaflet-measure/dist/leaflet-measure.css";
import "leaflet.zoomhome/dist/leaflet.zoomhome.js";

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  components: {
    LMap,
    LTileLayer,
    LControlScale,
    LMarker,
    LPopup,
    VueLeafletMinimap,
    VGeosearch,
    "v-locatecontrol": Vue2LeafletLocatecontrol,
  },

  data: () => ({
    center: [22.352, 114.15],
    zoom: 11,
    // siteMarkers: [
    //   { name: "TaiTam", location: [22.238, 114.226] },
    //   { name: "TingKok", location: [22.464, 114.216] },
    //   { name: "Capedaguilar", location: [22.206, 114.26] },
    //   { name: "Demo", location: [22.352, 114.15] },
    // ],
    content: '<iframe width="200" height="500" src="./myPopup.html"></iframe>',
    template:
      '<form id="popup-form">\
  <label for="input-speed">Input number:</label>\
  <input id="input-speed" class="popup-input" type="number" />\
  <table class="popup-table">\
    <tr class="popup-table-row">\
      <th class="popup-table-header">Arc numer:</th>\
      <td id="value-arc" class="popup-table-data">random number</td>\
    </tr>\
    <tr class="popup-table-row">\
      <th class="popup-table-header">Current speed:</th>\
      <td id="value-speed" class="popup-table-data">89198374198bdkjav</td>\
    </tr>\
  </table>\
  <button id="button-submit" type="button">Sample button</button>\
</form>',
    url:
      "http://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}",
    attribution:
      "Source &copy; Esri, DigitalGlobe, GeoEye, Earthstar Geographics, CNES/Airbus DS, USDA, USGS, AeroGRID, IGN, and the GIS User Community",
    minimapLayer: new L.TileLayer(
      "http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
    ),
    options: {
      position: "bottomright",
      zoomAnimation: true,
      width: 140,
      height: 125,
      toggleDisplay: true,
      minimize: true,
    },
    geosearchOptions: {
      provider: new OpenStreetMapProvider(),
    },
  }),

  methods: {
    openPopup: function (event) {
      this.$nextTick(() => {
        event.target.openPopup();
      });
    },
  },

  mounted() {
    this.$nextTick(() => {
      const map = this.$refs.map.mapObject;
      const measureControl = new window.L.Control.Measure({
        position: "topright",
        primaryLengthUnit: "meters",
        secondaryLengthUnit: "kilometers",
        primaryAreaUnit: "acres",
        secondaryAreaUnit: "hectares",
      });
      map.addControl(measureControl);

      const zoomHome = L.Control.zoomHome({
        position: "topleft",
      });
      map.addControl(zoomHome);
    });
  },
};
</script>

<style scoped>
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";
.popup-table {
  width: 100%;
}

.popup-table-row {
  background-color: grey;
}
.leaflet-popup-content-wrapper {
  width: 500px;
}
</style>