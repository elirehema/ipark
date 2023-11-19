<template>
  <v-container fluid>
    <v-row no-gutters>
      <v-col cols="12" sm="12" md="8">
        <div id="map-wrap" style="height: 100vh">
          <client-only>
            <l-map ref="myMap" :zoom="zoom" :center="center" @click="addMarker">
              <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
              <l-marker :lat-lng="marker"></l-marker>

              <l-geo-json :geojson="geojson" :options="options"> </l-geo-json>
            </l-map>
          </client-only>
        </div>
      </v-col>
      <v-col cols="12" md="4">
        <v-text-field
          outlined
          dense
          class="px-4"
          v-model="area"
          label="Area number"
          hint="Enter new area number"
          append-icon="mdi-send"
          @click:append="addnewarea()"
        ></v-text-field>
        <v-radio-group class="px-4" @change="changes" v-model="selectedArea">
          <v-radio
            v-for="group in groups"
            :key="group"
            :label="group"
            color="red"
            :value="group"
          >
            <template v-slot:append>
              <v-icon>mdi-send</v-icon>
            </template>
          </v-radio>
        </v-radio-group>

        <v-chip-group class="px-4" multiple active-class="primary--text">
          <v-chip
            v-for="tag in chips"
            :key="tag"
            close dense small
            @click="select"
            @click:close="remove(tag)"
          >
            {{ tag }}
          </v-chip>
        </v-chip-group>
        <pre class="text-caption px-4" >{{ data }}</pre>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import { LMap, LTileLayer, LGeoJson } from "vue2-leaflet";
import { ModalPlugin } from "bootstrap-vue";
import Vue from "vue";

Vue.use(ModalPlugin);
export default {
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
  },
  data() {
    return {
      url: "https://tile.openstreetmap.de/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      zoom: 15,
      center: [-6.769969, 38.718365],
      marker: [-6.7749896656540844, 38.71255541917059],
      soldColor: "#F3D2AC",
      activeColor: "#7DCF68",
      hoverColor: "#00B4EB",
      roadColor:"#4d254e",
      enableTooltip: true,
      allowClick: false,
      boxTwo: "",
      feature: null,
      area: null,
      selectedArea: null,
      selectedIndex: null,
      groups: [],
      chips: null,
      data: {
        value: [],
      },
      bounds: [
        [38.705213814595396, -6.770611256554568],
        [38.7221493220598, -6.7700279515987205],
      ],
      geojson: [],
    };
  },
  methods: {
    addnewarea() {
      if (this.area != null) {
        this.data[this.area] = [];
        this.groups.push(this.area);
        this.selectedArea = this.area;
      }
      this.area = null;
    },
    select: function (item) {
      if (this.selectedIndex == null) {
        this.selectedIndex = this.data[this.selectedArea].indexOf(item);
      } else {
        this.selectedIndex = null;
      }
    },
    changes: function (e) {
      this.chips = this.data[e];
    },
    remove: function (it) {
      var removedIndex = this.data[this.selectedArea].indexOf(it);
      this.data[this.selectedArea].splice(removedIndex, 1);
    },
    clicked: function () {
      console.log("CLICKED ");
    },
    addMarker(e) {
      var location = "[" + e.latlng.lng + "," + e.latlng.lat + "],";
      // console.log(e);
      navigator.clipboard.writeText(location);
      var value = [];
      value[0] = e.latlng.lng;
      value[1] = e.latlng.lat;
      this.$nextTick(() => {
       if(this.selectedArea != null){
        if (this.selectedIndex == null) {
          this.data[this.selectedArea].push(value);
        } else {
          this.data[this.selectedArea].splice(this.selectedIndex, 0, value);
        }
       }
      });
      console.log(this.data);

      // set marker at latlng position
      this.marker = e.latlng;

      // open popup on marker with a delay of 100 ms
     //setTimeout(() => this.$refs.marker.leafletObject.openPopup(), 100);
    },
    getColor: function (d) {
      return d > 1000
        ? "#800026"
        : d > 500
        ? "#BD0026"
        : d > 200
        ? "#E31A1C"
        : d > 100
        ? "#FC4E2A"
        : d > 50
        ? "#FD8D3C"
        : d > 20
        ? "#FEB24C"
        : d > 10
        ? "#FED976"
        : "#FFEDA0";
    },
    style: function (feature) {
      return {
        //fillColor: getColor(feature.properties.density),
        weight: 2,
        opacity: 1,
        color: "white",
        dashArray: "3",
        fillColor: "#63666a",
        fillOpacity: 0.7,
      };
    },
  },
  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction,
      };
    },
    datas() {
      return this.data;
    },
    styleFunction() {
      const fillColor = this.fillColor; // important! need touch fillColor in computed for re-calculate when change fillColor
      return () => {
        return {
          weight: 2,
          color: "white",
          opacity: 1,
          fillColor: "#63666a",
          fillOpacity: 1,
        };
      };
    },
    onEachFeatureFunction() {

      var activeColor = this.activeColor;
      var soldColor = this.soldColor;
      var hoverColor = this.hoverColor;
      var roadColor = this.roadColor;
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        
        layer.setStyle({
          weight: 1,
          color: "white",
          fillColor: feature.properties.type==0 ? roadColor : (feature.properties.status ? activeColor : soldColor),
          fillOpacity: 1,
        });
        layer.on("click", function (e) {
          // e = event
         if(this.allowClick){
          console.log("CLICKED FROM ON EACH");
          console.log(e);

          const h = $nuxt.$createElement;
          // Using HTML string
          const titleVNode = h("div", {
            domProps: { innerHTML: "<strong>Morden Industrial Park</strong>" },
          });
          // More complex structure
          const messageVNode = h("div", { class: ["foobar "] }, [
            h("p", { class: ["text-start"] }, [
              " Plot No.: ",
              h("strong", feature.properties.plot),
            ]),
            h("p", { class: ["text-start"] }, [
              " Size: ",
              h("strong", feature.properties.size + " SQM "),
            ]),
            h("p", { class: ["text-start"] }, [
              " Status : ",
              h("strong", feature.properties.status ? "Available" : "Not Available"),
            ]),
          ]);
          // We must pass the generated VNodes as arrays
          $nuxt.$bvModal.msgBoxConfirm([messageVNode], {
            title: [titleVNode],
            buttonSize: "md",
            okTitle: feature.properties.status ? "Purchase" : "Not Available",
            centered: true,
            size: "md",
          });
         }
         
        });

        layer.on("mouseover", function (e) {
          layer.setStyle({
            weight: 2,
            color: "white",
            stroke: true,
            fillColor: hoverColor,
            fillOpacity: 0.5,
          });
        });

        layer.on("mouseout", function (e) {
          layer.setStyle({
            weight: 2,
            color: "white",
            fillColor: feature.properties.status ? activeColor : soldColor,
            fillOpacity: 1,
          });
        });

        /**
        layer.bindTooltip(
          "<div>Plot No.:" +
            feature.properties.plot +
            "</div>" +
            "<div><strong>Size:" +
            feature.properties.size +
            " SQM</strong></div>" +
            "<div><strong>CMP: Modern Industrial Pack Mlandizi</strong></div>",
          { permanent: false, sticky: true }
        );
        **/
      };
    },
  },
  async created() {
    const response = await fetch("refined.geojson")
    const data = await response.json();
    this.geojson = data;
  }
};
</script>
<style scoped>
pre {
  display: block;
  font-family: monospace;
  font-size: small !important;
  color: blue;
  white-space: pre;
  margin: 1em 0;
}
</style>
