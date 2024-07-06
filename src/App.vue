<template>
  <div id="app">
    <MapGoogleOsmComponent/>
  </div>
</template>
<script>
  import MapGoogleOsmComponent from './components/MapGoogleOsmComponent.vue'
  export default {
    name: 'App',
    components: {
      MapGoogleOsmComponent
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
