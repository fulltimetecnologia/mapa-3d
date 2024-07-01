<template>
  <div>
    <div id="map" ref="map"></div>
  </div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

export default {
  name: 'MapComponent',
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
    await this.loadScript('https://cdn.osmbuildings.org/OSMBuildings-Leaflet.js');

    // Inicialização do Leaflet
    try {
      this.map = L.map(this.$refs.map).setView([-8.08, -34.90191], 13);
      L.tileLayer(this.layerMaps[this.layerMap], {
        maxZoom: 19,
      }).addTo(this.map);
      console.log('Leaflet initialized successfully');
    } catch (error) {
      console.error('Leaflet initialization error:', error);
    }

    // Inicialização do OSM Buildings
    try {
      new window.OSMBuildings(this.map).load();
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
