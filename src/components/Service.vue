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
    </div>
  </div>
  <div ref="serviceDetails" class="service-details">
    <p>
      Внимание! Представленные в таблицах услуги являются основными позициями из нашего прайс-листа.
      Точные цены могут варьироваться в зависимости от сложности работы, объёма услуг или
      особенностей объекта. Для уточнения стоимости и получения подробной консультации рекомендуем
      связаться с нами по телефону.
    </p>
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
                <div>{{ service.price }} ₽ {{ service.unit }}</div>
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
import lightbulb from '@/components/icons/lightbulb.png'
import socket from '@/components/icons/socket.png'
import bell from '@/components/icons/bell.png'
import tools from '@/components/icons/tools.png'
import drill from '@/components/icons/drill.png'
import wires from '@/components/icons/wires.png'
import meter from '@/components/icons/meter.png'
import cable from '@/components/icons/cable.png'
import other from '@/components/icons/other.png'

const items = ref([
  { id: 1, img_path: lightbulb, name: 'Освещение' },
  { id: 2, img_path: socket, name: 'Розетки/выключатели' },
  { id: 3, img_path: bell, name: 'Дверные звонки' },
  { id: 4, img_path: tools, name: 'Демонтаж' },
  { id: 5, img_path: drill, name: 'Штробление/сверление' },
  {
    id: 6,
    img_path: wires,
    name: 'Прокладка электрических проводов/кабелей',
  },
  { id: 7, img_path: meter, name: 'Соединительные коробки' },
  { id: 8, img_path: cable, name: 'Монтаж кабель канала' },
  { id: 9, img_path: other, name: 'Дополнительные услуги' },
])
const selectedService = ref(items.value[0].id)
const currentIndex = ref(0)
const serviceDetails = ref(null)
const titleElement = ref(null)
const gridContainer = ref(null)
const gridItems = ref([])
const tableTitleElement = ref(null)
const tableContent = ref(null)
const priceTable = ref(null)
const gridOther = ref(null)
let hasLoaded = ref(false)
const tables = ref([
  {
    title: 'Освещение',
    data: [
      { name: 'Монтаж и подключение точ.светильника', price: 300, unit: 'шт' },
      {
        name: 'Монтаж и подключение точ.светильника (гипсокартон, нат.потолок)',
        price: 290,
        unit: 'шт',
      },
      {
        name: 'Монтаж и подключение светильника типа "Амстронг"',
        price: 280,
        unit: 'шт',
      },
      {
        name: 'Монтаж и подключение накладного потолочного светильника',
        price: 400,
        unit: 'шт',
      },
      { name: 'Установка люстры', price: 600, unit: '' },
    ],
  },
  {
    title: 'Розетки/выключатели',
    data: [
      { name: 'Установка розетки/выключателя в готовое отверстие', price: 180, unit: 'шт' },
      { name: 'Установка интернет розетки в готовое отверстие', price: 200, unit: 'шт' },
      { name: 'Отверстие в гипсе и монтаж подрозетника', price: 170, unit: 'шт' },
      { name: 'Отверстие в бетоне и монтаж подрозетника', price: 300, unit: 'шт' },
      { name: 'Отверстие в кирпиче и монтаж подрозетника', price: 250, unit: 'шт' },
      { name: 'Демонтаж выключателя/розетки', price: 130, unit: 'шт' },
      { name: 'Демонтаж люстры, светильника', price: 300, unit: 'шт' },
      { name: 'Демонтаж патрона', price: 50, unit: 'шт' },
    ],
  },
  {
    title: 'Дверные звонки',
    data: [
      { name: 'Демонтаж старого звонка', price: 100, unit: 'шт' },
      { name: 'Установка дверного звонка или кнопки', price: 300, unit: 'шт' },
      {
        name: 'Установка дверного звонка на удалении от входа',
        price: 'от 1500',
        unit: 'шт',
      },
      { name: 'Монтаж и подключеие датчика движения', price: 'от 700', unit: 'шт' },
    ],
  },
  {
    title: 'Демонтаж',
    data: [
      { name: 'Демонтаж выключателя/розетки', price: 130, unit: 'шт' },
      { name: 'Демонтаж люстры, светильника', price: 300, unit: 'шт' },
      { name: 'Демонтаж патрона', price: 50, unit: 'шт' },
      { name: 'Демонтаж кабель-канала', price: 35, unit: 'м.п.' },
      { name: 'Демнтаж счетчика электроэнергии', price: 490, unit: 'шт' },
      { name: 'Демонтаж электропроводки', price: 35, unit: 'шт' },
      { name: 'Демонтаж старого звонка', price: 150, unit: 'шт' },
      { name: 'Демонтаж автомата', price: 100, unit: 'шт' },
    ],
  },
  {
    title: 'Штробление/сверление',
    data: [
      { name: 'Штробление по штукатурке и гипсу до 25х25мм.', price: 80, unit: 'п.м.' },
      { name: 'Отверстие в бетоне и монтаж подрозетника', price: 300, unit: 'шт' },
      { name: 'Отверстие в кирпиче и монтаж подрозетника', price: 250, unit: 'шт' },
      { name: 'Штробление по кирпичу и пенобетону до 25х25мм.', price: 120, unit: 'п.м.' },
      { name: 'Штробление по бетону/монолиту до 25х25мм.', price: 250, unit: 'п.м.' },
      {
        name: 'Сквозное сверление стен в газобетоне/пенобетоне',
        price: 'от 4',
        unit: 'см',
      },
      { name: 'Сквозное сверление стен в кирпиче', price: 'от 6', unit: 'см' },
      { name: 'Сквозное сверление стен в бетоне', price: 'от 11', unit: 'см' },
      { name: 'Отверстие под дюбель D= 6-10 мм.', price: 30, unit: 'шт' },
      { name: 'Отверстие под дюбель D= >10 мм.', price: '>50', unit: 'шт' },
      { name: 'Отверстие в гипсе и монтаж подрозетника', price: 170, unit: 'шт' },
    ],
  },
  {
    title: 'Прокладка электрических проводов/кабелей',
    data: [
      { name: 'Монтаж кабеля до 3х жил, сечением до 2,5мм 2', price: 70, unit: 'м.п.' },
      { name: 'Монтаж кабеля(1,5км.мм-4кв.мм) в гофре', price: 85, unit: 'м.п.' },
      { name: 'Монтаж UTP кабеля (интернет)', price: 35, unit: 'м.п.' },
      { name: 'Монтаж SAT кабеля (ТВ)', price: 35, unit: 'м.п.' },
    ],
  },
  {
    title: 'Соединительные коробки',
    data: [
      { name: 'Отверстие и монтаж коробки в гипсе и штукатурке', price: 170, unit: 'шт' },
      { name: 'Отверстие и монтаж коробки в кирпиче и пенобетоне', price: 250, unit: 'шт' },
      { name: 'Отверстие и монтаж коробки в бетоне и монолите', price: 300, unit: 'шт' },
      { name: 'Установка накладной коробки', price: 250, unit: 'шт' },
      { name: 'Установка накладной коробки', price: 200, unit: 'шт' },
    ],
  },
  {
    title: 'Монтаж кабель канала',
    data: [
      {
        name: 'Монтаж кабель-канала по гипсокартону и другим мягким материалам',
        price: 50,
        unit: 'м.п.',
      },
      { name: 'Монтаж кабель-канала по газобетону/пенобетону', price: 60, unit: 'м.п.' },
      { name: 'Монтаж кабель-канала по кирпичу', price: 90, unit: 'м.п.' },
      { name: 'Монтаж кабель-канала по бетону', price: 110, unit: 'м.п.' },
    ],
  },
  {
    title: 'Дополнительные услуги',
    data: [
      { name: 'Монтаж лотка металлического', price: 350, unit: 'м.п.' },
      { name: 'Монтаж лотка металлического', price: 'от 6000', unit: 'м.п.' },
    ],
  },
])

const selectService = (item) => {
  selectedService.value = item.id
  currentIndex.value = tables.value.findIndex((table) => table.title === item.name)

  priceTable.value.scrollIntoView({ behavior: 'smooth', block: 'center' })
}

const nextTable = () => {
  currentIndex.value = (currentIndex.value + 1) % tables.value.length
}

const prevTable = () => {
  currentIndex.value = (currentIndex.value - 1 + tables.value.length) % tables.value.length
}

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

p {
  margin: 10px 0;
  width: 60%;
  margin-left: auto;
  margin-right: auto;
  padding: 15px;
  font-family: Arial, sans-serif;
  color: #333;
  line-height: 1.6;
  font-size: 14px;
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
  padding-top: 1%;
  padding-bottom: 1%;
  color: #efebe2;
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
  table {
    max-width: 100%;
  }
}
@media (max-width: 768px) {
  p {
    width: 90%;
  }
  h1 {
    font-size: 30px;
  }
  .grid-container {
    grid-template-columns: repeat(2, minmax(150px, 1fr));
  }
  .other {
    grid-column: span 2;
  }
  .tableTitle {
    font-size: 22px;
    width: 70%;
  }
  .nav-button {
    font-size: 30px;
  }
  .fas {
    font-weight: bolder;
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
    font-size: 24px;
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
