<template>
    <div id="map-wrap" style="height: 100vh">
 <client-only>
   <l-map :zoom=13 :center="[-6.779932124032749, 38.71996284754632]">
     <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
     <l-marker :lat-lng="[-6.779932124032749, 38.71996284754632]"></l-marker>
     <l-geo-json :geojson="geojson"></l-geo-json>
   </l-map>
 </client-only>
</div>
</template>
<script>
import {LMap, LTileLayer, LGeoJson} from 'vue2-leaflet';

export default {
  components: {
    LMap,
    LTileLayer,
    LGeoJson
  },
  data () {
    return {
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:'&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 8,
      center: [47.313220, -1.319482],
      geojson: null
    };
  },
  async created () {
    const response = await fetch('https://raw.githubusercontent.com/gregoiredavid/france-geojson/master/regions.geojson');
    this.geojson = await response.json();
  }
}
</script>