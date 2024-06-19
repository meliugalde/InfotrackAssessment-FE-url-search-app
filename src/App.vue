<template>
  <div>
    <UrlSearch />
    <SearchHistory />
  </div>
</template>

<script>
import UrlSearch from './components/UrlSearch.vue';
import SearchHistory from './components/SearchHistory.vue';

export default {
  name: 'App',
  components: {
    UrlSearch,
    SearchHistory,
  },
};
</script>

<style>
/* Global styles */
body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  background-color: #f0f2f5;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app {
  width: 100%;
  max-width: 600px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
  border-radius: 10px;
  text-align: center;
}
</style>
