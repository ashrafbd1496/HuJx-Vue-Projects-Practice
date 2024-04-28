<script setup>
import { ref, onMounted } from "vue";

const quote = ref({});

const getQuotes = async () => {
  try {
    const response = await fetch(
      `https://api.quotable.io/quotes/random?tags=technology`
    );
    if (!response.ok) {
      throw new Error("Failed to fetch quote");
    }
    const data = await response.json();

    quote.value.content = data[0].content;
    quote.value.author = data[0].author;
  } catch (error) {
    console.error("Error fetching quotes:", error);
  }
};

// Call getQuotes function when component is mounted
onMounted(() => {
  getQuotes();
  setInterval(getQuotes, 5000);
});

const fetchNewQuote = () => {
  getQuotes();
};
</script>

<template>
  <div class="quote-generator">
    <h1 class="app-title">Random Quote Generator</h1>
    <blockquote class="quote-container">
      <p>"{{ quote.content }}"</p>
      <cite class="quote-author">_ _{{ quote.author }}</cite>
    </blockquote>
    <button @click="fetchNewQuote" class="quote-button">
      Wait or Click to get quote
    </button>
  </div>
</template>

<style scoped>
.quote-generator {
  border: solid 1px #fff;
  padding: 20px;
  display: grid;
  place-items: center end;
}
.quote-generator .app-title {
  border-bottom: 1px dashed #fff;
  margin-bottom: 20px;
}
.quote-container > cite {
  display: grid;
  text-align: right;
  margin-top: 10px;
}
.quote-generator .quote-button {
  display: inline-block;
  width: 150px;
  align-self: self-end;
  margin-top: 20px;
  padding: 5px;
}
</style>
