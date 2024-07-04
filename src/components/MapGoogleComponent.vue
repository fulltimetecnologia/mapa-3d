<template>
    <div>
      <div id="map" ref="map"></div>
    </div>
  </template>
  
  <script>
  import { Loader } from '@googlemaps/js-api-loader';
  import * as THREE from 'three';
  
  export default {
    name: 'MapGoogleComponent',
    data() {
      return {
        map: null,
        animationFrameId: null,
        layerMaps: [
          'https://api.maptiler.com/maps/dataviz/256/{z}/{x}/{y}@2x.png?key=GX4tcvbsx32DGx0ztZ3l',
          'https://api.maptiler.com/maps/bright-v2/256/{z}/{x}/{y}@2x.png?key=GX4tcvbsx32DGx0ztZ3l',
          'https://api.maptiler.com/maps/satellite/256/{z}/{x}/{y}@2x.jpg?key=GX4tcvbsx32DGx0ztZ3l',
        ],
        layerMap: 0,
      };
    },
    async mounted() {
      const apiOptions = {
        apiKey: 'AIzaSyDEP0W1aRUiHh3K8izlwk7zUWj6-XyotxY',
        version: 'beta',
        libraries: ['marker', 'places'],
      };
  
      const apiLoader = new Loader(apiOptions);
      await apiLoader.load();
  
      const mapOptions = {
        center: { lat: -8.08, lng: -34.90191 },
        zoom: 16,
        tilt: 30,
        heading: 0,
        mapId: 'd3b3f166493c990b',
        mapTypeId: 'satellite',
        mapTypeControl: true,
        zoomControl: false,
        fullscreenControl: false,
      };
  
      // eslint-disable-next-line no-undef
      this.map = new google.maps.Map(this.$refs.map, mapOptions);
  
      await this.initWebGLOverlayView();
  
      this.addAdvancedMarker({
        lat: -8.078538,
        lng: -34.894298,
        title: 'Custom Marker',
        type: 'custom',
      });
    },
    methods: {
      async initWebGLOverlayView() {
        let scene, renderer, camera;
        const webGLOverlayView = new google.maps.WebGLOverlayView();
  
        webGLOverlayView.onAdd = () => {
          scene = new THREE.Scene();
          camera = new THREE.PerspectiveCamera();
          const ambientLight = new THREE.AmbientLight(0xffffff, 0.75);
          scene.add(ambientLight);
          const directionalLight = new THREE.DirectionalLight(0xffffff, 0.25);
          directionalLight.position.set(0.5, -1, 0.5);
          scene.add(directionalLight);
        };
  
        // eslint-disable-next-line no-unused-vars
        webGLOverlayView.onContextRestored = ({ gl }) => {
          renderer = new THREE.WebGLRenderer({
            canvas: gl.canvas,
            context: gl,
            ...gl.getContextAttributes(),
          });
  
          renderer.autoClear = false;
  
          const animate = () => {
            // eslint-disable-next-line no-undef
            requestAnimationFrame(animate);
            this.map.moveCamera({
              tilt: 30,
              heading: this.map.getHeading() + 0.2,
              zoom: this.map.getZoom(),
            });
            renderer.render(scene, camera);
          };
          animate();
        };
  
        // eslint-disable-next-line no-unused-vars
        webGLOverlayView.onDraw = ({ gl, transformer }) => {
          webGLOverlayView.requestRedraw();
          renderer.render(scene, camera);
          renderer.resetState();
        };
  
        this.map.addListener('mousedown', () => cancelAnimationFrame(this.animationFrameId));
        this.map.addListener('touchstart', () => cancelAnimationFrame(this.animationFrameId));
  
        webGLOverlayView.setMap(this.map);
      },
      addAdvancedMarker(markerData) {
        const { lat, lng, title, type } = markerData;
        const backgroundColorMarker = '#f26722';
  
        const markerElement = document.createElement('div');
        markerElement.className = 'custom-marker';
        markerElement.style.backgroundColor = backgroundColorMarker;
        markerElement.style.width = '70px';
        markerElement.style.height = '70px';
        markerElement.style.borderRadius = '8px';
        markerElement.style.display = 'flex';
        markerElement.style.flexDirection = 'column';
        markerElement.style.alignItems = 'center';
        markerElement.style.justifyContent = 'center';
        markerElement.style.padding = '4px';
        markerElement.style.boxShadow = '0 2px 6px rgba(0,0,0,0.3)';
        markerElement.style.position = 'relative';
        markerElement.style.transition = 'transform 0.2s ease-in-out';
  
        const imgElement = document.createElement('div');
        imgElement.style.backgroundImage = `url(/images/${type}.png)`;
        imgElement.style.width = '32px';
        imgElement.style.height = '32px';
        imgElement.style.backgroundSize = 'contain';
        imgElement.style.backgroundRepeat = 'no-repeat';
        imgElement.style.backgroundPosition = 'center';
  
        const button = document.createElement('button');
        button.innerHTML = 'Ver detalhes >';
        button.style.display = 'none';
        button.style.marginTop = '5px';
        button.style.padding = '2px';
        button.style.fontSize = '0.55rem';
        button.style.fontWeight = '600';
        button.style.backgroundColor = '#FFFFFF';
        button.style.color = '#37434A';
        button.style.border = 'none';
        button.style.cursor = 'pointer';
        button.style.borderRadius = '4px';
  
        markerElement.appendChild(imgElement);
        markerElement.appendChild(button);
  
        markerElement.addEventListener('mouseover', () => {
          markerElement.style.transform = 'scale(1.2)';
          button.style.display = 'block';
        });
  
        markerElement.addEventListener('mouseout', () => {
          markerElement.style.transform = 'scale(1)';
          button.style.display = 'none';
        });
  
        button.addEventListener('click', () => {
          this.$root.$emit('marker-click', markerData);
        });
  
        // eslint-disable-next-line no-undef
        const marker = new google.maps.marker.AdvancedMarkerElement({
          position: { lat, lng },
          map: this.map,
          title,
          content: markerElement,
        });
  
        marker.addEventListener('gmp-click', () => {
          this.$root.$emit('marker-click', markerData);
        });
      },
    },
  };
  </script>
  
  <style>
  #map {
    width: 100%;
    height: 500px;
    position: relative;
  }
  </style>
  