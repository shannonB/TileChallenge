<template>
  <div id="app">
    <DisplayTiles/>
  </div>
</template>

<script>
import DisplayTiles from './components/DisplayTiles.vue';

export default {
  name: 'app',
  components: {
    DisplayTiles,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 10px;
}
</style>
