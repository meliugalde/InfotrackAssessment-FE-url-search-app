<template>
    <div>
        <!-- Button to Fetch Search History -->
      <button @click="fetchSearchHistory" class="btn btn-secondary">Search History</button>
      <button v-if="showSearchHistory" @click="toggleSearchHistory" class="btn btn-secondary">Hide Search History</button>
      <div v-if="showSearchHistory && searchHistory.length > 0" class="search-history">

    <h2>Search History</h2>
      <table class="table table-bordered">
        <thead>
          <tr>
            <th>URL</th>
            <th>Keywords</th>
            <th>Positions</th>
            <th>Search Date</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in searchHistory" :key="index">
            <td>{{ item.url }}</td>
            <td>{{ item.keywords }}</td>
            <td>{{ item.positions }}</td>
            <td>{{ formatSearchDate(item.searchDate) }}</td>
          </tr>
          <tr v-if="searchHistory.length === 0">
            <td colspan="4">No search history available.</td>
          </tr>
        </tbody>
      </table>
    </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        searchHistory: [],
        showSearchHistory: false,
      };
    },
    methods: {
      async fetchSearchHistory() {
        try {
            // TODO: extract url to config file
          const response = await axios.get('http://localhost:5005/search/history'); 
          this.searchHistory = response.data;
          this.showSearchHistory = true
        } catch (error) {
          console.error('Error fetching search history:', error);
        }
      },
      formatSearchDate(dateString) {
        const date = new Date(dateString);
        return date.toLocaleString(); 
      },
      toggleSearchHistory() {
      this.showSearchHistory = !this.showSearchHistory; 
    },
    }
  };
  </script>
  