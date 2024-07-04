<template>
  <div id="app">
    <!-- <MapGoogleComponent /><br> -->
    <MapGoogleSampleComponent /><br>
    <MapOsmComponent /><br>
    <MapLeafletComponent />
  </div>
</template>
<script>
/* import MapGoogleComponent from './components/MapGoogleComponent.vue' */
import MapGoogleSampleComponent from './components/MapGoogleSampleComponent.vue'
import MapOsmComponent from './components/MapOsmComponent.vue'
import MapLeafletComponent from './components/MapLeafletComponent.vue'
export default {
  name: 'App',
  components: {
    /* MapGoogleComponent, */
    MapGoogleSampleComponent,
    MapOsmComponent,
    MapLeafletComponent
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
