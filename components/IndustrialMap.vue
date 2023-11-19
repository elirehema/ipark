<template>
  <div id="map-wrap" style="height: 100vh">
    <b-modal centered id="modal-scoped">
      <template #modal-header="{ close }">
        <!-- Emulate built in modal header close button action -->
        
        <h5>Modal Header</h5>
      </template>

      <template #default="{ hide }">
        <p>Modal Body with button</p>
        <b-button @click="hide()">Hide Modal</b-button>
      </template>

      <template #modal-footer="{ ok, cancel, hide }">
        <b>Custom Footer</b>
        <!-- Emulate built in modal footer ok and cancel button actions -->
        <b-button size="sm" variant="success" @click="ok()"> OK </b-button>
        <b-button size="sm" variant="danger" @click="cancel()">
          Cancel
        </b-button>
        <!-- Button with custom close trigger value -->
        <b-button size="sm" variant="outline-secondary" @click="hide('forget')">
          Forget it
        </b-button>
      </template>
    </b-modal>
    <client-only>
      <l-map
        ref="myMap"
        :zoom="zoom"
        :center="center"
      >
        <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
        <l-marker :lat-lng="marker"></l-marker>

        <l-geo-json :geojson="geojson" :options="options">
        </l-geo-json>
      </l-map>
    </client-only>
  </div>
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
      marker: [-6.7749896656540844,38.71255541917059],
      soldColor: "#F3D2AC",
      activeColor: "#7DC8CA",
      hoverColor: "#00B4EB",
      roadColor:"#4d254e",
      enableTooltip: true,
      allowClick: false,
      boxTwo: "",
      feature: null,
      bounds: [
        [38.705213814595396, -6.770611256554568],
        [38.7221493220598, -6.7700279515987205],
      ],
      geojson: null,
    };
  },
  methods: {
    clicked: function () {
      console.log("CLICKED ");
    },
    addMarker(e) {
      var location = "[" + e.latlng.lng + "," + e.latlng.lat + "],";
      // console.log(e);
      navigator.clipboard.writeText(location);

      // set marker at latlng position
      this.marker = e.latlng;

      // open popup on marker with a delay of 100 ms
      setTimeout(() => this.$refs.marker.leafletObject.openPopup(), 100);
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
          if(feature.properties.type!=0){
          // e = event
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
          if(feature.properties.type!=0){
          layer.setStyle({
            weight: 2,
            color: "white",
            stroke: true,
            fillColor: hoverColor,
            fillOpacity: 0.5,
          });
        }
        });

        layer.on("mouseout", function (e) {
          if(feature.properties.type!=0){
          layer.setStyle({
            weight: 2,
            color: "white",
            fillColor:  feature.properties.status ? activeColor : soldColor,
            fillOpacity: 1,
          });
        }
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