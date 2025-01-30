<template>
  <div class="container">
    <img src="./icons/back-image.jpg" alt="background" />
    <div class="overlay"></div>
    <div ref="infoContainer" class="info-container">
      <p ref="titleElement" class="title fade-in delay-2">{{ title }}</p>
      <p ref="textElement" class="text fade-in delay-2">{{ text }}</p>
      <a
        ref="navLinkElement"
        :href="'#services'"
        class="nav-link fade-in delay-2"
        @click.prevent="scrollToSection('#services')"
      >
        Наши услуги
      </a>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { scrollToSection } from '@/utils/scroll'

const title = ref('Надежность, качество, профессионализм - это мы!')
const text = ref(
  'Наша компания занимается оказанием качественных услуг по монтажу электрики. Мы имеем ' +
    'многолетний опыт в сфере электромонтажа, что позволяет нам предлагать надёжные и современные ' +
    'решения для каждого клиента',
)

const infoContainer = ref(null)
const titleElement = ref(null)
const textElement = ref(null)
const navLinkElement = ref(null)

const setupIntersectionObserver = () => {
  const observer = new IntersectionObserver(
    (entries, observer) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible')
          observer.unobserve(entry.target)
        }
      })
    },
    {
      threshold: 0.01, // Срабатывает даже при 1% видимости
    },
  )

  ;[titleElement.value, textElement.value, navLinkElement.value].forEach((el) => {
    if (el) {
      observer.observe(el)

      if (el.getBoundingClientRect().top < window.innerHeight) {
        el.classList.add('visible')
      }
    }
  })
}

onMounted(() => {
  setupIntersectionObserver()
})
</script>

<style lang="scss" scoped>
.container {
  position: relative;
  height: 100vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 20px;
}

img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 0;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 1;
}

.info-container {
  position: relative;
  z-index: 2;
  max-width: 80%;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.title,
.text {
  margin: 0 auto;
}

.title {
  font-size: 64px;
  font-weight: 600;
  line-height: 1.2;
  margin-bottom: 20px;
  width: 80%;
  text-align: center;
}

.text {
  font-size: 20px;
  margin-bottom: 40px;
  width: 100%;
  text-align: center;
}

a {
  display: inline-block;
  background-color: #eeffee;
  color: #2f393d;
  text-decoration: none;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.3);
  text-transform: uppercase;
  padding: 15px 40px;
  border-radius: 5px;
  font-weight: 600;
  font-size: 18px;
  transition:
    background-color 0.3s ease,
    transform 0.3s ease;
}

a:hover {
  background-color: #d6f5d6;
  transform: scale(1.05);
}

.fade-in {
  opacity: 0;
  transform: translateY(30px);
  transition:
    opacity 1s ease,
    transform 1s ease;
}

.fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}

.delay {
  transition-delay: 0.5s;
}

.delay-2 {
  transition-delay: 1s;
}

@media (max-width: 1024px) {
  .title {
    font-size: 48px;
  }
  .text {
    font-size: 18px;
  }
  a {
    font-size: 16px;
    padding: 12px 30px;
  }
}

@media (max-width: 768px) {
  .title {
    font-size: 36px;
  }
  .text {
    font-size: 16px;
  }
  a {
    font-size: 14px;
    padding: 10px 25px;
  }
}

@media (max-width: 480px) {
  .title {
    width: 100%;
    font-size: 32px;
  }
  .text {
    font-size: 20px;
  }
  a {
    font-size: 20px;
    padding: 8px 20px;
  }
}
</style>
