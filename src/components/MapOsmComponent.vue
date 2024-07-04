<template>
    <div>
      <div id="map" ref="map"></div>
    </div>
  </template>
  
  <script>
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
        this.osmb.addMarker( {latitude: -8.078538,longitude: -34.894298,altitude: 10 + Math.random() * 50 }, {id: 1}, {icon: this.osmb.divIcon({className: this.getIconClass('Homicidio/Tentativa')})});  

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
      },
      getIconClass(type) {
        switch (type) {
        case 'Homicidio/Tentativa':
          return 'icon-marker-1'
        case 'Tentativa/Roubo':
          return 'icon-marker-2'
        case 'Briga':
          return 'icon-marker-3'
        case 'Ação policial':
          return 'icon-marker-4'
        case 'Ataque a civis':
          return 'icon-marker-5'
        case 'Não identificado':
          return 'icon-marker-6'
        case 'Operação policial':
          return 'icon-marker-7'
        case 'Sequestro/Cárcere Privado':
          return 'icon-marker-8'
        }
      },
    }
  };
  </script>
  
  <style>
  #map {
    width: 100%;
    height: 500px;
    position: relative;
  }
  
  .icon-marker-1 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: rgb(209, 53, 43);
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(209, 53, 43, 0.3);
}

.icon-marker-2 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: #52aba4;
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(82, 171, 164, 0.3);
}

.icon-marker-3 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: #b98859;
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(185, 136, 89, 0.3);
}

.icon-marker-4 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: rgb(23, 52, 207);
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(23, 52, 207, 0.3);
}

.icon-marker-5 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: rgb(231, 183, 8);
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(231, 183, 8, 0.3);
}

.icon-marker-6 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: rgb(117, 117, 117);
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(117, 117, 117, 0.3);
}
.icon-marker-7 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: rgb(232, 14, 203);
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(232, 14, 203, 0.3);
}
.icon-marker-8 {
    display: block;
    width: 5px;
    height: 5px;
    background-color: blueviolet;
    border-radius: 100%;
    box-shadow: 0px 0px 0px 5px rgba(138, 43, 226, 0.3);
}
  </style>
  