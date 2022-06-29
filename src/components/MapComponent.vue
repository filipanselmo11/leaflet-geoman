<template>
  <v-container>
    <v-card>
      <div id="map"></div>
    </v-card>
  </v-container>
</template>

<script>
import "leaflet/dist/leaflet.css";
import "leaflet";
import "@geoman-io/leaflet-geoman-free";
import "@geoman-io/leaflet-geoman-free/dist/leaflet-geoman.css";

const L = window["L"];

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  name: "MapComponent",
  data: () => ({
    map: null,
    tileLayer: null,
  }),
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      this.map = new L.Map("map", {
        pmIgnore: false,
        center: new L.LatLng(-25.441105, -49.276855),
        zoom: 5,
      });
      this.tileLayer = new L.tileLayer(
        "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
        {
          minZoom: 5,
          maxZoom: 18,
          attribution:
            '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
        }
      );
      this.map.addLayer(this.tileLayer);
      L.marker([-25.597253671992426, -49.14357997973205]).addTo(this.map);
      this.map.on("pm:create", (e) => {
        // e.layer.setStyle({ pmIgnore: false });
        L.PM.reInitLayer(e.layer);
      });
      this.map.pm.addControls({
        position: "bottomright",
        drawMarker: true,
        drawCircleMarker: true,
        drawPolyline: true,
        drawRectangle: true,
        drawPolygon: true,
        drawCircle: true,
        drawText: true,
        editMode: true,
        dragMode: true,
        cutPolygon: true,
        removalMode: true,
        rotateMode: true,
        oneBlock: false,
        drawControls: true,
        editControls: true,
        customControls: true,
        optionsControls: true,
        pinningOption: true,
        snappingOption: true,
        splitMode: true,
        scaleMode: true,
      });
    },
  },
};
</script>

<style scoped>
#map {
  height: 600px;
}
</style>
