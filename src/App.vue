<template>
  <div id="app">
    <AsyncSearch
      :items="countries"
      :isAsync="true"
    />
  </div>
</template>

<script>
import AsyncSearch from './components/AsyncSearch.vue';
import countries from './data/countries.json';

export default {
  name: 'App',
  components: {
    AsyncSearch
  },
  data() {
    return {
      countries: countries
    };
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
