<script setup>
import { ref } from "vue";

const searchQuery = ref("");
const searchResult = ref([]);
const isLoading = ref(false);
const error = ref(null);
const isDarkTheme = ref(false);

const searchWikipedia = async (query) => {
  const encodeQuery = encodeURIComponent(query);

  const endpoint = `https://bn.wikipedia.org/w/api.php?action=query&list=search&prop=info&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${encodeQuery}`;

  try {
    isLoading.value = true;
    const response = await fetch(endpoint);
    const data = await response.json();
    if (data.query && data.query.search) {
      searchResult.value = data.query.search;
      error.value = null;
    } else {
      searchResult.value = [];
      error.value = "No results found!";
    }
  } catch (err) {
    console.log("Error for fetching data: ", err);
    searchResult.value = [];
    error.value = "An Error occurred while fetchig data";
  } finally {
    isLoading.value = false;
  }
};

const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value;
};

const submitSearch = ()=> {
  if (searchQuery.value.trim() !== '') {
  searchWikipedia(searchQuery.value)    
  }else{
    searchResult.value = []
    error.value = 'Please enter a valid search term.'
  }
}
</script>
<template>
  <div :class="{'dark-theme' : isDarkTheme}">
   <div class="container">
    <div class="header-container">
      <h1>Search wikipedia</h1>
      <span id="theme-toggler" @click="toggleTheme">{{isDarkTheme ? 'Light' : 'Dark'}}</span>
    </div>

    <form @submit.prevent="submitSearch">
      <input type="text" v-model="searchQuery" placeholder="Enter the search term">
      <button type="submit">Search</button>
    </form>

    <div id="search-result">
      <div v-if="isLoading" class="spinner">Loading...</div>
      <p v-if="searchResult.length">
        <div v-for="(result) in searchResult" :key="result.pageid" class="result-item">
          <h3 class="result-title">
            <a :href="`https://bn.wikipedia.org/?curid=${result.pageid}`" target="_blank" rel="noopener">
              {{ result.title }}
            </a>
          </h3>
        </div>
      </p>
    </div>
   </div>
  </div>
</template>
<style scoped>
</style>
