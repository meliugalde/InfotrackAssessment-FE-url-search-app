<template>
    <div>
      <h1>Infotrack Url Search</h1>
      <form @submit.prevent="findUrlPositions">
        <div class="form-group">
          <label for="keywords">Keywords</label>
          <input type="text" id="keywords" v-model="keywords" class="form-control" placeholder="Enter keywords" />
        </div>
        <div class="form-group">
          <label for="targetUrl">Target URL</label>
          <input type="text" id="targetUrl" v-model="targetUrl" class="form-control" placeholder="Enter target URL" />
        </div>
        <button type="submit" class="btn btn-primary" :disabled="loading">Search</button>
      </form>
      <div v-if="searched">
        <h2>Results</h2>
        <div v-if="loading" class="loading">Searching...</div>
        <div v-else-if="error" class="error">{{ error }}</div>
        <div v-else-if="positions && positions.length > 0">
          Positions: {{ this.positions }}
        </div>
        <div v-else>No results found.</div>
      </div>
      
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import './styles.css';
  
  export default {
    data() {
      return {
        keywords: '',
        targetUrl: '',
        positions: [],
        searched: false,
        error: '',
        loading: false,
      };
    },
    methods: {
      async findUrlPositions() {
        this.searched = true;
        this.error = '';
        this.loading = true;
  
        if (!this.keywords) {
          this.error = 'Keywords are required';
          this.loading = false;
          return;
        }
  
        if (!this.targetUrl) {
          this.error = 'Target URL is required';
          this.loading = false;
          return;
        }
  
        this.positions = this.searchUrlPositions(this.keywords, this.targetUrl);
  
        if (this.positions.length === 0) {
          await this.fetchUrlPositionsFromAPI(this.keywords, this.targetUrl);
        }
  
        this.loading = false;
      },
      searchUrlPositions(keywords, targetUrl) {
        if (!keywords) {
          console.error('The keywords string is empty or null.');
          return [];
        }
  
        if (!targetUrl || targetUrl.trim() === '') {
          console.error('The target URL is null, empty, or contains only whitespace.');
          return [];
        }
  
        return keywords
          .split(',')
          .map((url, index) => ({ url: url.trim(), index }))
          .filter(x => x.url.toLowerCase().includes(targetUrl.toLowerCase()))
          .map(x => (x.index + 1).toString());
      },
      async fetchUrlPositionsFromAPI(keywords, targetUrl) {
        try {

            const queryParams = new URLSearchParams({
                keywords, 
                targetUrl
            }).toString();

           // TODO: extract url to config file
          const response = await axios.post(`http://localhost:5005/search/find-url-position?${queryParams}`);
          this.positions = response.data.searchDto.positions;
        } catch (error) {
          if (error.response) {
            console.error('Server error:', error.response.data);
            this.error = `Something went wrong. Please try again.`;
          } else if (error.request) {
            console.error('Network error:', error.request);
            this.error = 'Network error. Please check your connection.';
          } else {
            console.error('Error:', error.message);
            this.error = `Error: ${error.message}`;
          }
        }
      },
    },
  };
  </script>
  