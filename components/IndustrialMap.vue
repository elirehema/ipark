<template>
  <div id="map-wrap" style="height: 100vh">
    <client-only>
      <l-map :zoom="13" :center="[-6.769969, 38.718365]" @click="addMarker">
        <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
        <l-marker :lat-lng="[-6.769969, 38.718365]"></l-marker>
        <l-geo-json :geojson="features"></l-geo-json>
      </l-map>
    </client-only>
  </div>
</template>
<script>
import { LMap, LTileLayer, LGeoJson } from "vue2-leaflet";

export default {
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
  },
  data() {
    return {
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      zoom: 8,
      center: [-6.769969, 38.718365],
      geojson: null,
      features: [
        {
          type: "Feature",
          properties: { party: "Republican", color: 'green' },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.722207290952724, -6.770003053450008],
                [38.721538728515114, -6.772165282572815],
                [38.72151215657994, -6.772347166723752],
                [38.72152215172889, -6.77253372996141],
                [38.717139640332746, -6.772533122903327],
                [38.717025495665354, -6.770115331174424],
                [38.71959812878657, -6.7699388511182095],
                [38.72080478895482, -6.770040361573068],
                [38.722207290952724, -6.770003053450008],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: { party: "Republican" },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.72152215172889, -6.77253372996141],
                [38.721483162309674, -6.775504913208602],
                [38.717241239047674, -6.775504913208602],
                [38.717139640332746, -6.772533122903327],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: { party: "Republican" },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.717025495665354, -6.770115331174424],
                [38.716736804141505, -6.770136956345964],
                [38.715336544950425, -6.770444424342149],
                [38.714145116451924, -6.770530495490963],
                [38.71283779296874, -6.770450179011817],
                [38.71279941065889, -6.772016330628856],
                [38.71276247894484, -6.772128216531441],
                [38.712837533095936, -6.772650601081891],
                [38.717139640332746, -6.772533122903327],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: { party: "Republican" },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.71282712674068, -6.772645098322229],
                [38.71284805472938, -6.775409601573229],
                [38.717241239047674, -6.775504913208602],
                [38.717139640332746, -6.772533122903327],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: { party: "Republican" },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.72148991652563, -6.775985036411219],
                [38.71719294464111, -6.775910169199679],
                [38.71716112135862, -6.777780208146222],
                [38.72148420826056, -6.777764751963174],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: { party: "Republican" },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.72148420826056, -6.777764751963174],
                [38.721522206386645, -6.779766810240238],
                [38.72153791653951, -6.779821697490798],
                [38.72162375815952, -6.780257582999626],
                [38.721726018992676, -6.780438059733094],
                [38.71717076151138, -6.780310359115897],
                [38.71716112135862, -6.777780208146222],
                [38.72148420826056, -6.777764751963174],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: { party: "Republican" },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.717193189110404, -6.775915603062241],
                [38.71284269279576, -6.775787902153108],
                [38.71283773185143, -6.777833266314336],
                [38.71716112135862, -6.777780208146222],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: {
            party: "Republican",
            name: "2000000",
            show_on_map: false,
          },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.71284269031135, -6.777833366885536],
                [38.712842848321955, -6.780299942698772],
                [38.71717687672835, -6.780310520391406],
                [38.71716112135862, -6.777780208146222],
              ],
            ],
          },
        },
      ],
    };
  },
  methods: {
    addMarker(e) {
      var location = "[" + e.latlng.lng + "," + e.latlng.lat + "],";
      console.log(location);
      navigator.clipboard.writeText(location);

      // set marker at latlng position
      this.marker = e.latlng;

      // open popup on marker with a delay of 100 ms
      setTimeout(() => this.$refs.marker.leafletObject.openPopup(), 100);
    },
     getColor: function(d) {
    return d > 1000 ? '#800026' :
           d > 500  ? '#BD0026' :
           d > 200  ? '#E31A1C' :
           d > 100  ? '#FC4E2A' :
           d > 50   ? '#FD8D3C' :
           d > 20   ? '#FEB24C' :
           d > 10   ? '#FED976' :
                      '#FFEDA0';
},
     style:function(feature) {
    return {
        fillColor: getColor(feature.properties.density),
        weight: 2,
        opacity: 1,
        color: 'white',
        dashArray: '3',
        fillOpacity: 0.7
    };
}
  },
  async created() {
    const response = await fetch(
      "https://raw.githubusercontent.com/gregoiredavid/france-geojson/master/regions.geojson"
    );
    this.geojson = await response.json();
    LMap.geoJson(this.geoJson, {style: this.style}).addTo(map);
  },
};
</script>