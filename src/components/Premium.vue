<script setup>
import { ref } from 'vue'
import carImage from './icons/car.png'
import freeImage from './icons/free.png'
import individalImage from './icons/individual.png'
import clockImage from './icons/clock.png'
import documentImage from './icons/document.png'

const advantages = ref([
  { icon_path: carImage, text: 'Бесплатный выезд на оценку работ' },
  { icon_path: freeImage, text: 'Бесплатный расчёт стоимости работ' },
  { icon_path: clockImage, text: 'Соблюдение сроков' },
  { icon_path: documentImage, text: 'Официальный договор' },
  { icon_path: individalImage, text: 'Индивидуальный проект электрики' },
])
</script>

<template>
  <div class="container">
    <h1>Преимущества компании</h1>
    <p class="description">Мы перечислим несколько причин, почему вам стоит выбрать именно нас.</p>
    <div class="advantages-list">
      <div v-for="(adv, index) in advantages" :key="index" class="advantage">
        <img :src="adv.icon_path" />
        <p class="text">{{ adv.text }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
}

.container {
  background-color: #efebe2;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px 20px 90px;
  text-align: center;
}

h1 {
  margin: 0;
  padding-top: 30px;
  text-align: center;
  font-size: 42px;
  font-weight: 600;
}

.description {
  font-size: 20px;
  margin-bottom: 40px;
  width: 80%;
}

.advantages-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  width: 60%;
}

.advantage {
  display: flex;
  align-items: center;
  gap: 15px;
  flex: 1 1 250px;
  max-width: 300px;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
  background-color: white;
}

img {
  height: 80px;
  width: 80px;
}

.text {
  font-size: 18px;
  font-weight: bold;
  color: #000;
  margin: 0;
}

@media (max-width: 1024px) {
  h1 {
    font-size: 36px;
  }

  .description {
    font-size: 18px;
  }

  img {
    height: 70px;
    width: 70px;
  }

  .text {
    font-size: 16px;
  }
}

@media (max-width: 768px) {
  h1 {
    font-size: 30px;
  }

  .description {
    font-size: 16px;
  }

  img {
    height: 60px;
    width: 60px;
  }

  .text {
    font-size: 14px;
  }
}

@media (max-width: 480px) {
  h1 {
    font-size: 28px;
  }

  .description {
    font-size: 18px;
    width: 100%;
  }

  img {
    height: 50px;
    width: 50px;
  }

  .text {
    font-size: 16px;
    text-align: left;
  }

  .advantage {
    flex: 1 1 100%;
    max-width: 100%;
  }

  .advantages-list {
    width: 100%;
  }

  .container {
    padding: 20px 20px 90px;
  }
}
</style>
