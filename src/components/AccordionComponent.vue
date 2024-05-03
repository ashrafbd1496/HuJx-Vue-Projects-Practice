<script setup>
import { ref } from "vue";

const items = ref([
  {
    title: "What is HTML?",
    content:
      "HTML is the standard markup language for creating Web pages. What is HTML? HTML stands for Hyper Text Markup Language; HTML is the standard markup language ...",
    open: false,
  },
  {
    title: "What is JS?",
    content:
      "JavaScript is a scripting language that enables you to create dynamically updating content, control multimedia, animate images, and pretty much ...",
    open: false,
  },
  {
    title: "What is CSS?",
    content:
      "CSS stands for Cascading Style Sheets; CSS describes how HTML elements are to be displayed on ...",
    open: false,
  },
]);
const toggleAccordion = (index) => {
  items.value = items.value.map((item, i) => ({
    ...item,
    open: i === index ? !item.open : false,
  }));
};
</script>
<template>
  <div>
    <h1>Accordion</h1>
    <div class="accordion-container">
      <div v-for="(item, index) in items" :key="index" class="accordion">
        <div class="accordion-header" @click="toggleAccordion(index)">
          {{ item.title }}
          <span class="arrow-icon">{{ item.open ? "▼" : "▶" }}</span>
        </div>
        <div v-show="item.open" class="accordion-content">
          {{ item.content }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.accordion-container {
  max-width: 600px;
  margin: 50px auto;
}

.accordion {
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 10px;
}

.accordion-header {
  background-color: rgb(39, 153, 153);
  color: #fff;
  padding: 15px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 600px;
}

.arrow-icon {
  font-size: 20px;
}

.accordion-content {
  padding: 15px;
  display: none;
  border-top: 1px solid #ddd;
  background-color: #f9f9f9;
  color: #333;
}

.accordion-content[style] {
  display: block;
}
</style>
