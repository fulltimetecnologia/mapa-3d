<template>
  <div id="app">
    <MapOsmComponent />
    <br>
    <br>
    <MapLeafletComponent />
  </div>
</template>

<script>
import MapOsmComponent from './components/MapOsmComponent.vue'
import MapLeafletComponent from './components/MapLeafletComponent.vue'

export default {
  name: 'App',
  components: {
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
