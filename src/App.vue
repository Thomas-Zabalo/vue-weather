<template>
  <div id="app">
    <WeatherComponent />
  </div>
</template>

<script>
import WeatherComponent from './components/WeatherComponent.vue';

export default {
  name: 'App',
  components: {
    WeatherComponent
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
