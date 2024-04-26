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
body,html {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
}

.container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100%;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  font-size: 3rem;
  margin-bottom: 2rem;

}
#header-container{
  flex: 1;
}
#search-form {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 2rem;
}

#search-input {
  font-size: 1.2rem;
  padding: 0.5rem 1rem;
  margin-right: 1rem;
  border: 2px solid #ccc;
  border-radius: 0.25rem;
  flex-grow: 1;
}

#search-input:focus {
  outline: none;
  border-color: #0074d9;
}

button[type='submit'] {
  padding: 0.3rem 1rem;
  background-color: #0074d9;
  color: #fff;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
}

button[type='submit']:hover {
  background-color: #0063ad;
}

#search-results {
  margin-bottom: 2rem;
}

.result-item {
  margin-bottom: 1rem;
}

.result-title {
  font-size: 1.5rem;
  margin-top: 0;
}

.result-link {
  display: block;
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
  color: #0074d9;
}

.result-link:hover {
  text-decoration: underline;
}

.result-snippet {
  margin-top: 0;
}

.spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  height: 10rem;
}

/* Dark theme */
.header-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

#theme-toggler {
  border: none;
  background: transparent;
  cursor: pointer;
  background: #e2e2e2;
  padding: 10px 20px;
  border-radius: 100px;
}

.dark-theme {
  background-color: #282c34;
  color: #fff;
}

.dark-theme #search-input {
  background-color: #454545;
  color: #fff;
  border-color: #fff;
}

.dark-theme #search-input:focus {
  border-color: #0074d9;
}

.dark-theme button[type='submit'] {
  background-color: #0074d9;
}

.dark-theme .result-link,
.dark-theme .result-link:hover {
  color: #90caf9;
}
</style>
