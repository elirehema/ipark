<template>
  <div>
    <l-map :zoom="zoom" :center="center" style="height: 500px; width: 100%">
      <template v-for="pol in pols">
        <l-polygon
          :lat-lngs="pol.coordinates"
          :key="pol.plot"
          :color="pol.color"
        >
          <l-popup content="Polygon" />
        </l-polygon>
      </template>
    </l-map>
  </div>
</template>
  
  <script>
import {
  LMap,
  LTileLayer,
  LCircle,
  LRectangle,
  LPolygon,
  LPolyline,
  LPopup,
  LTooltip,
} from "vue2-leaflet";
import { latLng } from "leaflet";

export default {
  name: "PopupGeometryTest",
  components: {
    LMap,
    LTileLayer,
    LCircle,
    LRectangle,
    LPolygon,
    LPolyline,
    LPopup,
    LTooltip,
  },
  data() {
    return {
      zoom: 11,
      center: [47.31322, -1.319482],
      circle: {
        center: latLng(47.41322, -1.0482),
        radius: 4500,
      },
      rectangle: {
        bounds: [
          [47.341456, -1.397133],
          [47.303901, -1.243813],
        ],
        color: "red",
      },
      pols: [
        {
          plot: "373",
          size: "95,006",
          color: "#ff00ff",
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
      ],
      polygon: {
        latlngs: [
          [47.2263299, -1.6222],
          [47.21024000000001, -1.6270065],
          [47.1969447, -1.6136169],
          [47.18527929999999, -1.6143036],
          [47.1794457, -1.6098404],
          [47.1775788, -1.5985107],
          [47.1676598, -1.5753365],
          [47.1593731, -1.5521622],
          [47.1593731, -1.5319061],
          [47.1722111, -1.5143967],
          [47.1960115, -1.4841843],
          [47.2095404, -1.4848709],
          [47.2291277, -1.4683914],
          [47.2533687, -1.5116501],
          [47.2577961, -1.5531921],
          [47.26828069, -1.5621185],
          [47.2657179, -1.589241],
          [47.2589612, -1.6204834],
          [47.237287, -1.6266632],
          [47.2263299, -1.6222],
        ],
        color: "#ff00ff",
      },
      polyline: {
        type: "polyline",
        latlngs: [
          [47.334852, -1.509485],
          [47.342596, -1.328731],
          [47.241487, -1.190568],
          [47.234787, -1.358337],
        ],
        color: "green",
      },
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
    };
  },
};
</script>