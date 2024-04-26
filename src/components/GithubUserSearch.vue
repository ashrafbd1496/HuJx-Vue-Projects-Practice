<script setup>
import { ref } from "vue";

const username = ref("");
const userprofile = ref(null);
const error = ref(null);

const getUserProfile = async () => {
  try {
    // Check if username is empty
    if (!username.value.trim()) {
      userprofile.value = null;
      error.value = null; // No error if username is empty
      return; // Exit the function early
    }
    
    // If the username is not empty, continue with fetching data
    const response = await fetch(
      `https://api.github.com/users/${username.value}`
    );

    const data = await response.json();

    if (response.ok) {
      userprofile.value = data;
      error.value = null;
    } else {
      userprofile.value = null;
      error.value = `Error: ${data.message}`;
    }
  } catch (err) {
    console.log("Error fetching data:", err);
    error.value = "An error occurred when fetching data";
  }
};

</script>

<template>
  <div class="container">
    <header class="header">
      <h1>Github User Search App</h1>
    </header>

    <div class="content">
      <main>
        <h3>Github User Profile</h3>
        <div class="input-contianer">
          <input
            v-model="username"
            placeholder="Enter github username"
            @input="getUserProfile"
          />
        </div>
        <div v-if="userprofile" class="user-profile">
          <img :src="userprofile.avatar_url" :alt="userprofile.login" />
          <div class="user-details">
            <p><strong>Name: </strong>{{ userprofile.login }}</p>
            <p><strong>Loaction: </strong>{{ userprofile.location }}</p>
            <p><strong>Followers: </strong>{{ userprofile.followers }}</p>
            <p><strong>Following: </strong>{{ userprofile.following }}</p>
            <p><strong>Public Repos: </strong>{{ userprofile.public_repos }}</p>
          </div>
        </div>
        <div v-if="error" class="error-message">
          <p>{{ error }}</p>
        </div>
      </main>
    </div>

    <footer class="footer">All Rights &copy; Reserved</footer>
  </div>
</template>

<style scoped>
body,
html {
  margin: 0;
  width: 100vw;
}
.container {
  height: 100vh;
  width: 100vw;
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: auto 1fr auto;
  background-color: darkcyan;
  color: #fff;
  grid-template-areas:
    "header"
    "content"
    "footer";
}
.header {
  display: grid;
  width: 100%;
  grid-area: header;
  justify-content: center;
  border-bottom: 2px solid #fff;
  padding: 10px;
  min-height: 100px;
}
.content {
  display: grid;
  grid-area: content;
  justify-content: center;
  margin-top: 50px;
}
.footer {
  display: grid;
  grid-area: footer;
  place-items: center;
  justify-content: center;
  min-height: 100px;
}
.input-container {
  padding: 5px;
  height: 10px;
  border-radius: 5px;
}
</style>
