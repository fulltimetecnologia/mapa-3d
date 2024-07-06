<template>
  <div>
    <div id="map" ref="map"></div>
  </div>
</template>

<script>

import { Loader } from '@googlemaps/js-api-loader';
import * as THREE from 'three';

export default {
  name: 'MapGoogleOsmComponent',
  data() {
    return {
      map: null,
      markers: [],
      osmb: null,
      animationFrameId: null,
    };
  },
  async mounted() {

    await this.loadScript('https://cdn.osmbuildings.org/4.1.1/OSMBuildings.js');

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
      mapId: 'd3b3f166493c990b',
      mapTypeId: 'satellite',
      mapTypeControl: true,
      zoomControl: true,
      fullscreenControl: true,
    };

    this.map = new google.maps.Map(this.$refs.map, mapOptions);

    await this.initWebGLOverlayView();

    // Adiciona marcadores padrão com cores diferentes
    this.addMarker({ lat: -8.078538, lng: -34.894298, color: '#f26722' });
    this.addMarker({ lat: -8.079, lng: -34.895, color: '#00ff00' });
    this.addMarker({ lat: -8.08, lng: -34.896, color: '#0000ff' });

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

        this.osmb = new window.OSMBuildings({ minZoom: 15, maxZoom: 20}); 
        this.osmb.addGeoJSONTiles('https://{s}-data.onegeo.co/maps/tiles/{z}/{x}/{y}.json?token=5se76u6aj5ycdbpq', { fixedZoom: 15 });
        scene.add(this.osmb);

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
          this.animationFrameId = requestAnimationFrame(animate);
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
        try {
          renderer.render(scene, camera);
          renderer.resetState();
        } catch (error) {
          if (error.message.includes('ResizeObserver loop')) {
            console.warn('ResizeObserver loop error ignored.');
          } else {
            throw error;
          }
        }
      };

      this.map.addListener('mousedown', () => cancelAnimationFrame(this.animationFrameId));
      this.map.addListener('touchstart', () => cancelAnimationFrame(this.animationFrameId));

      webGLOverlayView.setMap(this.map);
    },
    addMarker({ lat, lng, color, path }) {
      const marker = new google.maps.Marker({
        position: { lat, lng },
        map: this.map,
        icon: {
          path: path || google.maps.SymbolPath.CIRCLE,
          fillColor: color,
          fillOpacity: 1,
          strokeColor: '#ffffff',
          strokeWeight: 2,
          scale: 10,
        },
      });

      this.markers.push(marker);
    },
    loadScript(src) {
      return new Promise((resolve, reject) => {
        const script = document.createElement('script');
        script.src = src;
        script.onload = () => resolve();
        script.onerror = () => reject(new Error(`Script load error for ${src}`));
        document.head.appendChild(script);
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
