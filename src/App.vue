<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { VueSpinner } from 'vue3-spinners';

import SvgImage from '/src/images/pattern-divider-desktop.svg';
import Dice from '/src/images/icon-dice.svg';

const items = ref({});
const isLoading = ref(true);

const fetchAdvice = async () => {
  isLoading.value = true;
  try {
    const response = await axios.get('https://api.adviceslip.com/advice');
    items.value = response.data.slip;
  } catch (error) {
    console.error("Error fetching data:", error);
  } finally {
    isLoading.value = false;
  }
};

onMounted(fetchAdvice);
</script>

<template>
  <div >
    <div v-if="isLoading" class="load">
      <VueSpinner size="100" color="#52ffa8" />
    </div>
    <div v-else class="container">
      <span class="title">ADVICE# {{ items.id }}</span>
      <span class="quote">"{{ items.advice }}"</span>
      <img :src="SvgImage" alt="divider" />
      <div class="button" @click="fetchAdvice">
        <img class="dice" :src="Dice" alt="dice button" />
      </div>
    </div>
  </div>
</template>


<style scoped>
.load {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}
.container {
  width: auto;
  display: flex;
  flex-direction: column;
  position: relative;
  background-color: #323a49;
  border-radius: 10px;
  padding: 20px;
  padding-bottom: 64px;
  gap: 24px;
  max-width: 500px;
  
}
.title {
  text-align: center;
  font-size: 15px;
  letter-spacing: 5px;
  font-weight: 600;
  color: #52ffa8;
}
.quote {
  color: #cde3e9;
  font-size: 28px;
  font-weight: 700;
  text-align: center;
}
.dice {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.button {
  border-radius: 100%;
  background: #52ffa8;
  width: 4rem;
  height: 4rem;
  position: absolute;
  left: 50%;
  bottom: -30px;
  transform: translateX(-50%);
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}
.button:hover {
  box-shadow: 0px 0px 30px rgba(82, 255, 168, 0.8);
}
</style>
