<template>
    <div id="map-wrap" style="height: 100vh">
 <client-only>
   <l-map :zoom=13 :center="[-6.779932124032749, 38.71996284754632]">
     <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
     <l-marker :lat-lng="[-6.779932124032749, 38.71996284754632]"></l-marker>
   </l-map>
 </client-only>
</div>
</template>