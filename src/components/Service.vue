<template>
  <div class="container">
    <h1 class="fade-in">Категории услуг</h1>
    <div class="grid-container">
      <div
        class="grid-item fade-in"
        v-for="(item, index) in items"
        :key="item.id"
        :style="{ animationDelay: `${index * 0.2}s` }"
        @click="selectService(item)"
      >
        <a>
          <img :src="item.img_path" alt="service-icon" />
        </a>
        <h4>{{ item.name }}</h4>
      </div>
    </div>
  </div>
  <div ref="serviceDetails" class="service-details">
    <div class="nav-buttons">
      <button class="nav-button left" @click="prevTable" aria-label="Previous Table">
        <i class="fas fa-chevron-left"></i>
      </button>
      <h1 class="tableTitle">{{ tables[currentIndex].title }}</h1>
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

<script setup>
import { ref, onMounted } from 'vue'
import lightbulbImage from './icons/lightbulb.png'
import socketImage from './icons/socket.png'
import bellImage from './icons/bell.png'
import toolsImage from './icons/tools.png'
import drillImage from './icons/drill.png'
import meterImage from './icons/meter.png'
import { useSwipe } from '@vueuse/core'

const items = ref([
  { id: 1, img_path: lightbulbImage, name: 'Освещение' },
  { id: 2, img_path: socketImage, name: 'Розетки/выключатели' },
  { id: 3, img_path: bellImage, name: 'Дверные звонки' },
  { id: 4, img_path: toolsImage, name: 'Демонтаж' },
  { id: 5, img_path: drillImage, name: 'Штробление/сверление' },
  {
    id: 6,
    img_path: meterImage,
    name: 'Монтаж, установка и подключение счётчика электроэнергии',
  },
])

const selectedService = ref(items.value[0].id)
const currentIndex = ref(0)
const container = ref(null)
const serviceDetails = ref(null)

const selectService = (item) => {
  selectedService.value = item
  loadServiceTables(item.id)
  scrollToServiceDetails()
}

const scrollToServiceDetails = () => {
  if (serviceDetails.value) {
    const offset = 72
    const rect = serviceDetails.value.getBoundingClientRect()
    window.scrollTo({
      top: window.scrollY + rect.top - offset,
      behavior: 'smooth',
    })
  }
}

const loadServiceTables = (id) => {
  const Tables = [
    {
      title: 'Освещене',
      data: [
        { name: 'Маникюр', price: 1200 },
        { name: 'Стрижка', price: 1500 },
        { name: 'Массаж', price: 2500 },
      ],
    },
    {
      title: 'Розетки/выключатели',
      data: [
        { name: 'Педикюр', price: 1800 },
        { name: 'Окрашивание', price: 2200 },
        { name: 'Спа-процедуры', price: 3000 },
      ],
    },
    {
      title: 'Дверные звонки',
      data: [
        { name: 'Укладка', price: 1300 },
        { name: 'Брови и ресницы', price: 1100 },
        { name: 'Эпиляция', price: 2500 },
      ],
    },
    {
      title: 'Демонтаж',
      data: [
        { name: 'Укладка', price: 1300 },
        { name: 'Брови и ресницы', price: 1100 },
        { name: 'Эпиляция', price: 2500 },
      ],
    },
    {
      title: 'Штробление/сверление',
      data: [
        { name: 'Укладка', price: 1300 },
        { name: 'Брови и ресницы', price: 1100 },
        { name: 'Эпиляция', price: 2500 },
      ],
    },
    {
      title: 'Монтаж, установка и подключение счётчика электроэнергии',
      data: [
        { name: 'Укладка', price: 1300 },
        { name: 'Брови и ресницы', price: 1100 },
        { name: 'Эпиляция', price: 2500 },
      ],
    },
  ]
  tables.value = Tables
  currentIndex.value = id ? id - 1 : 0
}

const tables = ref([])

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

onMounted(() => {
  document.querySelectorAll('.fade-in').forEach((el) => {
    el.classList.add('visible')
  })
})

loadServiceTables(items.value[0].id)
</script>

<style scoped>
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
}

.container {
  background-color: #efebe2;
  padding: 60px 20px;
  text-align: center;
  min-height: 70vh;
}

h1 {
  font-size: 42px;
  font-weight: 600;
  margin-bottom: 40px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  max-width: 1000px;
  margin: 0 auto;
}

.grid-item {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
  transition:
    transform 0.3s ease,
    box-shadow 0.3s ease;
  opacity: 0;
  transform: translateY(30px);
  cursor: pointer;
}

.grid-item:hover {
  transform: translateY(-5px);
  box-shadow: 0px 8px 20px rgba(0, 0, 0, 0.2);
}

a img {
  height: 80px;
  width: 80px;
  margin-bottom: 15px;
}

h4 {
  font-size: 20px;
  font-weight: 600;
}

.tableTitle {
  text-align: center;
  width: 55%;
  color: #fff;
  font-size: 32px;
  font-weight: bold;
  margin: 20px 0;
}

.fade-in {
  transition:
    opacity 0.8s ease-out,
    transform 0.8s ease-out;
}

.visible {
  opacity: 1;
  transform: translateY(0);
}

.service-details {
  background: #2f393d;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
  min-height: 80vh;
  padding-top: 5%;
  color: #fff;
}

.nav-buttons {
  display: flex;
  flex-direction: row;
  justify-content: center;
  width: 100%;
  box-sizing: border-box;
  position: relative;
  margin-bottom: 20px;
}

.nav-button {
  background: none;
  border: none;
  color: #fff;
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
  max-width: 800px;
  border-collapse: collapse;
  border-radius: 8px;
  overflow: hidden;
  background: #efebe2;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

td {
  font-size: 20px;
  padding: 15px 20px;
  display: flex;
  justify-content: space-between;
  color: #333;
  border-bottom: 1px solid black;
  background: #efebe2;
}

@media (max-width: 1024px) {
  h1 {
    font-size: 36px;
  }

  .grid-container {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }

  table {
    max-width: 100%;
  }
}

@media (max-width: 768px) {
  h1 {
    font-size: 30px;
  }

  .grid-container {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  }

  .tableTitle {
    font-size: 22px;
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
    font-size: 28px;
  }

  .grid-container {
    grid-template-columns: 1fr;
  }

  .tableTitle {
    font-size: 28px;
  }

  .nav-button {
    font-size: 18px;
  }

  td {
    font-size: 18px;
  }

  a img {
    height: 60px;
    width: 60px;
  }

  h4 {
    font-size: 24px;
    margin-top: 10px;
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
