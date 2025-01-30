<script setup>
import { ref } from 'vue'
import { useSwipe } from '@vueuse/core'

const tables = ref([
  {
    title: 'Услуги по маникюру и стрижке',
    data: [
      { name: 'Маникюр', price: 1200 },
      { name: 'Стрижка', price: 1500 },
      { name: 'Массаж', price: 2500 },
      { name: 'Маникюр', price: 1200 },
      { name: 'Стрижка', price: 1500 },
      { name: 'Массаж', price: 2500 },
      { name: 'Маникюр', price: 1200 },
      { name: 'Стрижка', price: 1500 },
      { name: 'Массаж', price: 2500 },
    ],
  },
  {
    title: 'Процедуры для ухода',
    data: [
      { name: 'Педикюр', price: 1800 },
      { name: 'Окрашивание', price: 2200 },
      { name: 'Спа-процедуры', price: 3000 },
      { name: 'Маникюр', price: 1200 },
      { name: 'Стрижка', price: 1500 },
      { name: 'Массаж', price: 2500 },
    ],
  },
  {
    title: 'Дополнительные услуги',
    data: [
      { name: 'Укладка', price: 1300 },
      { name: 'Брови и ресницы', price: 1100 },
      { name: 'Эпиляция', price: 2500 },
      { name: 'Маникюр', price: 1200 },
      { name: 'Стрижка', price: 1500 },
      { name: 'Массаж', price: 2500 },
    ],
  },
])
const currentIndex = ref(0)
const container = ref(null)

const nextTable = () => {
  currentIndex.value = (currentIndex.value + 1) % tables.value.length
}

const prevTable = () => {
  currentIndex.value = (currentIndex.value - 1 + tables.value.length) % tables.value.length
}

useSwipe(container, {
  onSwipeLeft: nextTable,
  onSwipeRight: prevTable,
})
</script>

<template>
  <div class="container" ref="container">
    <div class="nav-buttons">
      <button class="nav-button left" @click="prevTable" aria-label="Previous Table">
        <i class="fas fa-chevron-left"></i>
      </button>
      <h1>{{ tables[currentIndex].title }}</h1>
      <button class="nav-button right" @click="nextTable" aria-label="Next Table">
        <i class="fas fa-chevron-right"></i>
      </button>
    </div>
    <div class="content">
      <transition name="fade" mode="out-in">
        <table v-if="tables[currentIndex]" :key="currentIndex">
          <tbody>
            <tr v-for="(service, idx) in tables[currentIndex].data" :key="idx">
              <td>
                <div>{{ service.name }}</div>
                <div>{{ service.price }} ₽</div>
              </td>
            </tr>
          </tbody>
        </table>
      </transition>
    </div>
  </div>
</template>

<style scoped>
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  background-color: #f9f9f9;
  text-align: center;
  overflow: hidden;
  padding-top: 50px;
}

.nav-buttons {
  display: flex;
  flex-direction: row;
  justify-content: center;
  width: 100%;
  box-sizing: border-box;
  position: relative;
}

.nav-button {
  background: none;
  border: none;
  color: #333;
  cursor: pointer;
  transition:
    transform 0.2s,
    color 0.2s;
  padding: 0 15px;
  outline: none;
  font-size: 24px;
}

.nav-button:hover {
  color: #007bff;
  transform: scale(1.1);
}

h1 {
  color: #333;
  font-size: 28px;
  font-weight: bold;
  flex-grow: 1;
  text-align: center;
  margin: 0;
  padding: 0 20px;
}

.content {
  display: flex;
  justify-content: center;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
  margin-top: 40px;
}

table {
  width: 100%;
  max-width: 600px;
  border-collapse: collapse;
  border-radius: 8px;
  overflow: hidden;
  background: #fff;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

td {
  font-size: 18px;
  padding: 15px 20px;
  display: flex;
  justify-content: space-between;
  color: #333;
  border-bottom: 1px solid #e0e0e0;
  background: #fff;
}

td:last-child {
  border-bottom: none;
}

@media (max-width: 768px) {
  h1 {
    font-size: 24px;
  }

  .nav-button {
    font-size: 20px;
  }

  td {
    font-size: 16px;
  }
}

@media (max-width: 480px) {
  h1 {
    font-size: 20px;
  }

  .nav-button {
    font-size: 18px;
  }

  td {
    font-size: 14px;
  }

  table {
    max-width: 100%;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
