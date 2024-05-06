<template>
  <div>
    <PasswordGenerator />
  </div>
</template>
<script setup>
import { ref } from "vue";
import PasswordGenerator from "./components/PasswordGenerator.vue";
</script>
<style>
body,
html {
  margin: 0;
  padding: 0;
  background-color: #008080;
  color: #fff;
}
</style>
