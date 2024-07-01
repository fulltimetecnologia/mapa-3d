<template>
  <div>
    <div id="map" ref="map"></div>
  </div>
</template>

<script>

import 'leaflet/dist/leaflet.css'

export default {
  name: 'MapOsmComponent',
  data() {
    return {
      map: null,
      osmb: null,
      layerMaps: [
        'https://api.maptiler.com/maps/dataviz/256/{z}/{x}/{y}@2x.png?key=GX4tcvbsx32DGx0ztZ3l',
        'https://api.maptiler.com/maps/bright-v2/256/{z}/{x}/{y}@2x.png?key=GX4tcvbsx32DGx0ztZ3l',
        'https://api.maptiler.com/maps/satellite/256/{z}/{x}/{y}@2x.jpg?key=GX4tcvbsx32DGx0ztZ3l',
      ],
      layerMap: 0,
    };
  },
  async mounted() {

    await this.loadScript('https://cdn.osmbuildings.org/4.1.1/OSMBuildings.js');

    try {

      this.osmb = new window.OSMBuildings({
        container: this.$refs.map,
        position: { latitude: -8.08, longitude: -34.90191 },
        tilt: 30,
        zoom: 16,
        minZoom: 15,
        maxZoom: 20,
        attribution: '© Map & Geo Data <a href="https://openstreetmap.org/copyright/">OpenStreetMap</a> © 3D <a href="https://osmbuildings.org/copyright/">OSM Buildings</a>'
      });

      this.osmb.addMapTiles(this.layerMaps[this.layerMap]);
      this.osmb.addGeoJSONTiles('https://{s}-data.onegeo.co/maps/tiles/{z}/{x}/{y}.json?token=5se76u6aj5ycdbpq', { fixedZoom: 15 });

      console.log('OSM Buildings initialized successfully');

    } catch (error) {
      console.error('OSM Buildings initialization error:', error);
    }
  },
  methods: {
    loadScript(src) {
      return new Promise((resolve, reject) => {
        const script = document.createElement('script');
        script.src = src;
        script.onload = () => resolve();
        script.onerror = () => reject(new Error(`Script load error for ${src}`));
        document.head.appendChild(script);
      });
    }
  }
};
</script>

<style>
#map {
  width: 100%;
  height: 500px;
}
</style>
