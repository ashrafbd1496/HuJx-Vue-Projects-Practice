<script setup>
import { ref } from "vue";

const passwordLength = ref(12);
const includeUppercase = ref(true);
const includeLowercase = ref(true);
const includeNumbers = ref(true);
const includeSymbols = ref(true);
const generatedPassword = ref("");

const generatePassword = () => {
  const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
  const uppercaseChars = includeUppercase.value
    ? "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    : "";
  const numberChars = includeNumbers.value ? "0123456789" : "";
  const symbolChars = includeSymbols.value ? "!@#$%^&*()_+[]{}|;:,.<>?/~`" : "";

  const allChars = lowercaseChars + uppercaseChars + numberChars + symbolChars;

  let password = "";
  for (let i = 0; i < passwordLength.value; i++) {
    const randomIndex = Math.floor(Math.random() * allChars.length);
    password += allChars[randomIndex];
  }

  generatedPassword.value = password;
};
</script>

<template>
  <div class="password-generator-container">
    <h2 class="password-generator-title">Password Generator</h2>
    <div v-if="generatedPassword" class="generated-password">
      {{ generatedPassword }}
    </div>
    <div class="setting">
      <label for="length">Password Length: </label>
      <input
        type="range"
        id="length"
        v-model="passwordLength"
        min="4"
        max="32"
      />
    </div>
    <div class="setting">
      <label for="includeUppercase">Include Uppercase:</label>
      <input type="checkbox" id="includeUppercase" v-model="includeUppercase" />
    </div>
    <div class="setting">
      <label for="includeLowercase">Include Lowercase:</label>
      <input type="checkbox" id="includeLowercase" v-model="includeLowercase" />
    </div>
    <div class="setting">
      <label for="includeNumbers">Include Numbers:</label>
      <input type="checkbox" id="includeNumbers" v-model="includeNumbers" />
    </div>
    <div class="setting">
      <label for="includeSymbols">Include Symbols:</label>
      <input type="checkbox" id="includeSymbols" v-model="includeSymbols" />
    </div>

    <button @click="generatePassword" class="generate-button">
      Generate Password
    </button>
  </div>
</template>

<style scoped>
.password-generator-container {
  --field-color: rgba(255, 255, 255, 0.08);
  max-width: 400px;
  margin: 50px auto;
  background-color: #0a0e31;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.45), 0 4px 8px rgba(0, 0, 0, 0.35),
    0 8px 12px rgba(0, 0, 0, 0.15);
}

.password-generator-title {
  font-size: 28px;
  color: #fff;
  margin-bottom: 20px;
}
.password-generator-container .setting {
  display: flex;
  flex-direction: row;
  align-items: center;
  background-color: var(--field-color);
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
}

label {
  flex: 70;
}
input {
  flex: 30;
  padding: 8px;
  margin: 10px;
}

.generate-button {
  width: 100%;
  padding: 10px 15px;
  margin-top: 20px;
  font-size: 16px;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  text-transform: uppercase;
  transition: all 150ms ease;
  background-image: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  letter-spacing: 1px;
}

.generated-password {
  background-color: rgba(255, 255, 255, 0.08);
  margin: 20px 10px;
  padding: 15px;
  border-radius: 10px;
  color: #fff;
}
</style>
