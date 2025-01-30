<script setup>
import Header from '../components/Header.vue'
import Hero from '../components/Hero.vue'
import Service from '../components/Service.vue'
import PriceList from '@/components/PriceList.vue'
import Premium from '@/components/Premium.vue'
import Contacts from '@/components/Contacts.vue'

const scrollToSection = (id) => {
  const section = document.querySelector(id)
  if (section) {
    section.scrollIntoView({ behavior: 'smooth', block: 'start' })
    isMenuOpen.value = false
  }
}
</script>

<template>
  <Header />
  <main>
    <div id="hero"><Hero /></div>
    <div id="premium"><Premium /></div>
    <div id="services"><Service /></div>
    <div id="contacts"><Contacts /></div>
  </main>
</template>

<style scoped>
html {
  scroll-behavior: smooth;
}
</style>
