<template>
  <div id="app">
    <MapGoogleComponent /><br>
    <MapOsmComponent /><br>
    <MapLeafletComponent /><br>
    <!-- <MapGoogleOsmComponent /> -->
  </div>
</template>
<script>
  import MapGoogleComponent from './components/MapGoogleComponent.vue'
  import MapOsmComponent from './components/MapOsmComponent.vue'
  import MapLeafletComponent from './components/MapLeafletComponent.vue'
  //import MapGoogleOsmComponent from './components/MapGoogleOsmComponent.vue'
  export default {
    name: 'App',
    components: {
      MapGoogleComponent,
      MapOsmComponent,
      MapLeafletComponent,
      //MapGoogleOsmComponent
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
