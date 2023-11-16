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
        @click="addMarker"
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
      zoom: 14,
      center: [-6.769969, 38.718365],
      marker: [-6.7749896656540844,38.71255541917059],
      fillColor: "#005163",
      hoverColor: "#3b4876",
      enableTooltip: true,
      boxTwo: "",
      feature: null,
      bounds: [
        [38.705213814595396, -6.770611256554568],
        [38.7221493220598, -6.7700279515987205],
      ],
      geojson: [
        {
          type: "Feature",
          properties: {
            plot: "373",
            size: "95,006",
            density: 20,
            status: 1,
          },
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
          properties: {
            plot: "379",
            size: "104,047",
            status: 1,
          },
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
          properties: {
            plot: "372",
            size: "110,969",
            status: 0,
          },
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
          properties: {
            plot: "378",
            size: "103,471",
            status: 1,
          },
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
          properties: {
            plot: "381",
            size: "100,595",
            status: 1,
          },
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
          properties: {
            plot: "384",
            size: "76,100",
            status: 0,
          },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.72148420826056, -6.777764751963174],
                [38.721522206386645, -6.779766810240238],
                [38.72153791653951, -6.779821697490798],
                [38.72162375815952, -6.780257582999626],
                [38.721726018992676, -6.780438059733094],
                [38.71714990209335, -6.78041775240851],

                [38.71716112135862, -6.777780208146222],
                [38.72148420826056, -6.777764751963174],
              ],
            ],
          },
        },
        {
          type: "Feature",
          properties: {
            plot: "380",
            size: "83,150",
            status: 1,
          },
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
            plot: "382",
            size: "71,888",
            status: 0,
          },
          geometry: {
            type: "Polygon",
            coordinates: [
              [
                [38.71284269031135, -6.777833366885536],
                [38.712879563978106, -6.780617862303952],
                [38.71714990209335, -6.78041775240851],
                [38.71716112135862, -6.777780208146222],
              ],
            ],
          },
        },
      ],
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
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.setStyle({
          weight: 1,
          color: "white",
          fillColor: feature.properties.status ? "#6fa8dd" :"#e94064",
          fillOpacity: 1,
        });
        layer.on("click", function (e) {
          // e = event
          console.log("CLICKED FROM ON EACH");
          console.log(e);

          const h = $nuxt.$createElement
        // Using HTML string
        const titleVNode = h('div', { domProps: { innerHTML: "<strong>Morden Industrial Park</strong>" } })
        // More complex structure
        const messageVNode = h('div', { class: ['foobar '] }, [
        h('p', { class: ['text-start'] }, [
            ' Plot No.: ',
            h('strong', feature.properties.plot),
          ]),
          h('p', { class: ['text-start'] }, [
            ' Size: ',
            h('strong', feature.properties.size+ " SQM "),
          ]),
          h('p', { class: ['text-start'] }, [
            ' Status : ',
            h('strong', feature.properties.status ? 'Available':'Not Available' ),
          ]),
        ])
        // We must pass the generated VNodes as arrays
        $nuxt.$bvModal.msgBoxConfirm([messageVNode], {
          title: [titleVNode],
          buttonSize: 'md',
          okTitle: feature.properties.status ? 'Purchase':'Not Available',
          centered: true, size: 'md'
        })
          /** 
          $nuxt.$bvModal.show('modal-scoped')
          
          $nuxt.$bvModal
            .msgBoxOk(`Size: ${feature.properties.size} SQM \n Status: Available`, {
              title: "Modern Industrial Pack Mlandizi",
              size: "md",
              buttonSize: "sm",
              okVariant: "danger",
              okTitle: "YES",
              cancelTitle: "NO",
              footerClass: "p-2",
              hideHeaderClose: false,
              centered: true,
            })
            .then((value) => {
              this.boxTwo = value;
            })
            .catch((err) => {
              // An error occurred
            });
            **/
          // You can make your ajax call declaration here
          //$.ajax(...
        });

        layer.on("mouseover", function (e) {
          layer.setStyle({
            weight: 2,
            color: "white",
            stroke: true,
            fillColor: "#3b4876",
            fillOpacity: 0.5,
          });
        });

        layer.on("mouseout", function (e) {
          layer.setStyle({
            weight: 2,
            color: "white",
            fillColor: feature.properties.status ? "#6fa8dd" :"#e94064",
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
};
</script>