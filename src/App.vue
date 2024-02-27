<template>
  <div id="app">
    <div class="search-container">
      <h1>Async Search</h1>
      <AsyncSearch
        :items="countries"
        :isAsync="true"
        class="search-box"
        @toggle-selection="toggleSelection"
      />
      <p>With country name and code</p>
      <h2>Sync Search</h2>
      <AsyncSearch 
        :items="countries"
        labelKey="name"
        class="search-box"
        @toggle-selection="toggleSelection"
      />
      <p>With country name only</p>
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
      countries: countries,
      selectedCountries: []
    };
  },
  methods: {
    // Method to toggle selection and update selectedCountries array
    toggleSelection(result) {
      const index = this.selectedCountries.findIndex(
        item => item.name === result.name && item.code === result.code
      );
      if (index !== -1) {
        this.selectedCountries.splice(index, 1);
      } else {
        this.selectedCountries.push(result);
      }
    },
  },
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
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
} 

p {
  color: #333;
  font-size: small;
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.search-box {
  margin-bottom: 1px;
  width: 100%;
}

.search-content {
  margin-bottom: 20px;
}

.search-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 300px;
  margin: 0 auto;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 20px;
  background-color: #E8E8E8;
}


</style>
