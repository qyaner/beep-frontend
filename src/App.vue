<template>
  <div id="app">
    <div class="search-container">
      <h1>Async Search</h1>
      <AsyncSearch
        :items="countries"
        :isAsync="true"
        class="search-box"
      />
      <h2>Sync Search</h2>
      <AsyncSearch 
        :items="countries"
        class="search-box"
      />
    </div>
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
body {
  background-color: #336699;
}
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  margin-top: 250px;
}

h1, h2 {
  color: #333;
  font-size: large;
}

.search-box {
  margin-bottom: 20px;
  width: 100%;
}

.search-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 220px;
  width: 300px;
  margin: 0 auto;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 20px;
  background-color: #E8E8E8;
}


</style>
