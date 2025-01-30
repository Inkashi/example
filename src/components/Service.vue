<template>
  <div class="container">
    <h1 ref="titleElement" class="fade-in">Категории услуг</h1>
    <div ref="gridContainer" class="grid-container">
      <div
        v-for="(item, index) in items"
        :key="item.id"
        ref="gridItems"
        class="grid-item fade-in"
        :style="{ transitionDelay: hasLoaded ? `0.1s` : `${index * 0.2}s` }"
        @click="selectService(item)"
      >
        <a>
          <img :src="item.img_path" alt="service-icon" />
        </a>
        <h4>{{ item.name }}</h4>
      </div>
      <div
        class="grid-item fade-in other"
        ref="gridOther"
        :style="{ transitionDelay: hasLoaded ? `0.1s` : `1.2s` }"
      >
        <a @click="selectService(other)"><h4>Дополнительные услуги</h4></a>
      </div>
    </div>
  </div>
  <div ref="serviceDetails" class="service-details">
    <div class="nav-buttons">
      <button class="nav-button left" @click="prevTable" aria-label="Previous Table">
        <i class="fas fa-chevron-left"></i>
      </button>
      <h1 ref="tableTitleElement" class="tableTitle fade-in">
        {{ tables[currentIndex]?.title }}
      </h1>
      <button class="nav-button right" @click="nextTable" aria-label="Next Table">
        <i class="fas fa-chevron-right"></i>
      </button>
    </div>
    <div ref="tableContent" class="content fade-in">
      <transition name="fade" mode="out-in">
        <table v-if="tables[currentIndex]" :key="currentIndex" ref="priceTable">
          <tbody>
            <tr v-for="(service, idx) in tables[currentIndex].data" :key="idx">
              <td>
                <div class="serviceName">{{ service.name }}</div>
                <div>{{ service.price }} ₽/{{ service.unit }}</div>
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

const other = { id: 7, img_path: lightbulbImage, name: 'Дополнительные услуги' }
const selectedService = ref(items.value[0].id)
const currentIndex = ref(0)
const container = ref(null)
const serviceDetails = ref(null)
const titleElement = ref(null)
const gridContainer = ref(null)
const gridItems = ref([])
const tableTitleElement = ref(null)
const tableContent = ref(null)
const priceTable = ref(null)
const gridOther = ref(null)
let hasLoaded = ref(false)

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
      title: 'Освещение',
      data: [
        { name: 'Монтаж и подкл. точечного светильника', price: 80, unit: 'шт' },
        {
          name: 'Монтаж и подкл.точечного светильника (гипсокартон, натяжной потолок)',
          price: 1500,
          unit: 'шт',
        },
        { name: 'Монтаж и подключение светильника типа "Амстронг"', price: 2500, unit: 'шт' },
        { name: 'Монтаж и подключение накладного потолочного светильника', price: 80, unit: 'шт' },
      ],
    },
    {
      title: 'Розетки/выключатели',
      data: [
        { name: 'Штробление по штукатурке и гипсу до 25х25мм.', price: 80, unit: 'п.м' },
        { name: 'Штробление по кирпичу и пенобетону до 25х25мм.', price: 120, unit: 'п.м' },
        { name: 'Штробление по бетону/монолиту до 25х25мм', price: 250, unit: 'п.м' },
        { name: 'Отверстие в гипсе и монтаж подрозетника', price: 170, unit: 'шт' },
        { name: 'Отверстие в бетоне и монтаж подрозетника', price: 300, unit: 'шт' },
        { name: 'Отверстие в кирпиче и монтаж подрозетника', price: 250, unit: 'шт' },
        { name: 'Отверстие под дюбель D= 6-10 мм. ', price: 30, unit: '' },
        { name: 'Отверстие под дюбель D= >10 мм. ', price: 50, unit: '' },
        { name: 'Установка розетки/выключателя в готовое отверстие', price: 180, unit: 'шт' },
        { name: 'Установка интернет розетки в готовое отверстие', price: 200, unit: 'шт' },
      ],
    },
    {
      title: 'Дверные звонки',
      data: [
        { name: 'Демонтаж старого звонка', price: 100, unit: 'шт' },
        { name: 'Установка дверного звонка или кнопки', price: 300, unit: 'шт' },
        { name: 'Установка дверного звонка на удалении от входа', price: 'от 1500', unit: 'шт' },
        { name: 'Монтаж и подключение датчика движения', price: 700, unit: 'шт' },
      ],
    },
    {
      title: 'Демонтаж',
      data: [
        { name: 'Демонтаж выключателя/розетки ', price: 130, unit: 'шт' },
        { name: 'Демонтаж люстры, светильника', price: 300, unit: 'шт' },
        { name: 'Демонтаж патрона', price: 50, unit: 'шт' },
        { name: 'Демонтаж кабель-канала', price: 35, unit: 'п.м' },
        { name: 'Демнтаж счетчика электроэнергии', price: 490, unit: 'шт' },
        { name: 'Демонтаж электропроводки', price: 35, unit: 'п.м' },
      ],
    },
    {
      title: 'Штробление/сверление',
      data: [
        { name: 'Монтаж кабеля до 3х жил, сечением до 2,5мм 2', price: 70, unit: 'п.м' },
        { name: 'Монтаж кабеля(1,5км.мм-4кв.мм) в гофре', price: 85, unit: 'п.м' },
        { name: 'Монтаж UTP кабеля (интернет) ', price: 35, unit: 'п.м' },
        { name: 'Монтаж SAT кабеля (ТВ) ', price: 35, unit: 'п.м' },
      ],
    },
    {
      title: 'Монтаж, установка и подключение счётчика электроэнергии',
      data: [
        { name: 'Отверстие и монтаж коробки в гипсе и штукатурке', price: 170, unit: 'шт' },
        { name: 'Отверстие и монтаж коробки в кирпиче и пенобетоне', price: 250, unit: 'шт' },
        { name: 'Отверстие и монтаж коробки в бетоне и монолите', price: 300, unit: 'шт' },
        { name: 'Установка накладной коробки ', price: 250, unit: 'шт' },
        { name: 'Соединение проводов в коробке', price: 200, unit: 'шт' },
        { name: 'По гипсокартону и другим мягким материалам', price: 50, unit: 'п.м' },
        { name: 'По газобетону/пенобетону', price: 60, unit: 'п.м' },
        { name: 'По кирпичу ', price: 90, unit: 'п.м' },
        { name: 'По бетону', price: 110, unit: 'п.м' },
      ],
    },
    {
      title: 'Дополнительные услуги',
      data: [
        { name: 'Монтаж лотка металлического ', price: 350, unit: 'п.м' },
        { name: 'Монтаж и сборка электрощита ', price: 'от 6000', unit: '' },
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
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible')
          observer.unobserve(entry.target)
          setTimeout(() => {
            hasLoaded.value = true
          }, 1000)
        }
      })
    },
    {
      threshold: 0.2,
    },
  )
  if (titleElement.value) observer.observe(titleElement.value)
  if (gridContainer.value) observer.observe(gridContainer.value)
  if (tableTitleElement.value) observer.observe(tableTitleElement.value)
  if (tableContent.value) observer.observe(tableContent.value)
  if (priceTable.value) observer.observe(priceTable.value)
  if (gridItems.value && gridItems.value.length > 0) {
    gridItems.value.forEach((item) => observer.observe(item))
  }
  if (gridOther.value) observer.observe(gridOther.value)
})

loadServiceTables(items.value[0].id)
</script>

<style scoped lang="scss">
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
}
.container {
  background-color: #2f393d;
  padding: 60px 20px;
  text-align: center;
  min-height: 70vh;
}
h1 {
  color: #efebe2;
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
    box-shadow 0.3s ease,
    opacity 0.8s ease-out;
  opacity: 0;
  transform: translateY(30px);
  cursor: pointer;
}
.grid-item.visible {
  opacity: 1;
  transform: translateY(0);
}
.grid-item:hover {
  transform: translateY(-5px);
  box-shadow: 0px 8px 20px rgba(0, 0, 0, 0.2);
}

.other {
  grid-column: span 3;
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
  color: #2f393d;
  font-size: 32px;
  font-weight: bold;
  margin: 20px 0;
}
.service-details {
  background: #efebe2;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
  min-height: 80vh;
  padding-top: 5%;
  color: #efebe2;
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
  color: #2f393d;
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
  opacity: 0;
  box-sizing: border-box;
  margin-top: 40px;
  transition:
    opacity 0.8s ease-out,
    transform 0.8s ease-out;
}
table {
  width: 100%;
  max-width: 800px;
  border-collapse: collapse;
  border-radius: 8px;
  overflow: hidden;
  background: #2f393d;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  opacity: 1;
  transition:
    opacity 0.8s ease-out,
    transform 0.8s ease-out;
}
.content.visible {
  opacity: 1;
}
td {
  font-size: 20px;
  padding: 15px 20px;
  display: flex;
  justify-content: space-between;
  color: #efebe2;
  border-bottom: 1px solid #efebe2;
  background: #2f393d;
}

td .serviceName {
  text-align: left;
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

  .other {
    grid-column: span 1;
    height: 40px;
    text-align: center;
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
