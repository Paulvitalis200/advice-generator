<script setup>
import axios from 'axios';
import { onMounted, ref, reactive } from 'vue';
import {
  ContentLoader
} from 'vue-content-loader'


const adviceObject = reactive({
  id: null,
  advice: ""
})
const loading = ref(false);

onMounted(() => {
  fetchAdvice()
})

const fetchAdvice = async () => {
  loading.value = true
    const response = await axios.get("https://api.adviceslip.com/advice")
    adviceObject.id = response.data.slip.id
    adviceObject.advice = response.data.slip.advice
    loading.value = false
}
</script>

<template>
  <main>
    <div class="advice">
      <div v-if="!loading">
        <div class="advice-container">
          <p class="heading">ADVICE #{{ adviceObject.id }} </p>
        <p class="advice-body">"{{ adviceObject?.advice }}"</p>
       
        </div>
        <img src="./assets/pattern-divider-desktop.svg" class="desktop-divider"/>
        <img src="./assets/pattern-divider-mobile.svg" class="mobile-divider"/>
        
        <div class="dice" @click="fetchAdvice">
          <img src="./assets/icon-dice.svg" />
        </div>
     </div>
      <div v-else>
        <ContentLoader viewBox="0 0 250 110" class="desktop-loader">
          <rect x="90" y="10" rx="3" ry="3" width="70" height="10" />
          <rect x="15" y="30" rx="3" ry="3" width="220" height="10" />
          <rect x="32" y="50" rx="3" ry="3" width="190" height="10" />
          <rect x="50" y="70" rx="3" ry="3" width="150" height="10" />
        </ContentLoader>

        <ContentLoader viewBox="0 0 250 110" class="mobile-loader">
          <rect x="90" y="10" rx="3" ry="3" width="70" height="20" />
          <rect x="15" y="40" rx="3" ry="3" width="220" height="20" />
          <rect x="32" y="70" rx="3" ry="3" width="190" height="20" />
         
        </ContentLoader>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  background-color: #202733;
  height: 100vh;
  width: 100vw;
  padding: 0;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.advice {
  width: 540px;
  min-height: 332px;
  border-radius: 15px;
  background-color: #313A48;
  padding: 48px;
  text-align: center;
  font-family: "Open Sans", sans-serif;
}

.heading {
  color: #53FFAA;
  font-weight: 10px;
}

.advice-body {
  font-size: 30px;
  font-weight: 700;
  color: white;
}

.dice {
  position: relative;
  top: 110px;
  background-color: #53FFAA;
  padding: 17px 15px 15px 15px;
  border-radius: 50%;
  left: 44%;
  width: 30px;
  cursor: pointer;
}

.advice-container {
  height: 220px;
  overflow: scroll;
  overflow-y: hidden; /* Hide vertical scrollbar */
  overflow-x: hidden; /* Hide horizontal scrollbar */
}

.mobile-divider {
  display: none;
}

.desktop-divider {
  display: block;
  width: 100%;
}

.desktop-loader {
  display: block;
}

.mobile-loader {
  display: none;
}
@media only screen and (max-width: 500px) {
  .advice {
    width: 300px;
  }

  .desktop-divider {
  display: none;
}

.mobile-divider {
  display: block;
}

.dice {
  left: 120px;
  top: 80px;
}

.desktop-loader {
  display: none;
}

.mobile-loader {
  display: block;
}

.advice-container {
  height: 300px;
  margin-bottom: 20px;
 
}
}
</style>
