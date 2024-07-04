<template>
  <div>
    <div id="map" ref="map"></div>
  </div>
</template>

<script>

import * as L from 'leaflet';
import 'leaflet/dist/leaflet.css';

export default {
  name: 'MapLeafletComponent',
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

    const map = new L.Map(this.$refs.map, {
      zoomControl: true,
      attributionControl: false
    });

    map.setView([-8.08, -34.90191], 16, false);

    new L.TileLayer(this.layerMaps[this.layerMap], {
      attribution: '© Map & Geo Data <a href="https://openstreetmap.org/copyright/">OpenStreetMap</a> © 3D <a href="https://osmbuildings.org/copyright/">OSM Buildings</a>',
      maxZoom: 18,
      maxNativeZoom: 20
    }).addTo(map);

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
