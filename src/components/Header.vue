<template>
  <header class="header">
    <nav class="nav">
      <button class="burger-menu" @click="toggleMenu">☰</button>
      <ul :class="['nav-list', { active: isMenuOpen }]" ref="navList">
        <li v-for="(link, index) in links" :key="index" class="nav-item">
          <a :href="link.url" class="nav-link" @click.prevent="scrollToSection(link.url)">
            {{ link.text }}
          </a>
        </li>
      </ul>
    </nav>
    <h1>VectorHM</h1>
  </header>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { scrollToSection } from '@/utils/scroll'

const isMenuOpen = ref(false)
const links = ref([
  { text: 'Главная', url: '#hero' },
  { text: 'Преимущества', url: '#premium' },
  { text: 'Услуги', url: '#services' },
  { text: 'Контакты', url: '#contacts' },
])

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

const navList = ref(null)

onMounted(() => {
  const handleResize = () => {
    if (window.innerWidth > 768) {
      isMenuOpen.value = true
    } else {
      isMenuOpen.value = false
    }
  }

  window.addEventListener('resize', handleResize)
  handleResize()
})
</script>

<style scoped>
.header {
  width: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(10px);
  z-index: 1000;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  padding-right: 5%;
  color: white;
}

.nav-list {
  list-style: none;
  display: flex;
  margin: 0;
  padding: 0;
  transition: all 0.3s ease-in-out;
}

.nav-item {
  margin-left: 20px;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-size: 20px;
  padding: 10px;
  transition: color 0.3s ease;
}

.nav-link:hover {
  color: #a9c4ce;
}

.burger-menu {
  display: none;
  background: none;
  border: none;
  color: white;
  font-size: 28px;
  cursor: pointer;
}

@media (max-width: 768px) {
  .burger-menu {
    display: block;
    margin-left: 2vw;
    margin-top: 5%;
    margin-right: auto;
    font-size: 40px !important;
  }

  .header {
    justify-content: space-between;
    align-items: start;
  }

  .nav-list {
    margin-right: auto;
    margin-left: auto;
    flex-direction: column;
    width: 100%;
    height: 0;
    overflow: hidden;
    transition: height 0.3s ease-in-out;
  }

  .nav-list.active {
    height: auto;
    max-height: 400px;
  }

  .nav-item {
    margin: 10px 0;
  }

  .nav-link {
    font-size: 20px;
  }
  .burger-menu {
    font-size: 30px;
  }
}
</style>
