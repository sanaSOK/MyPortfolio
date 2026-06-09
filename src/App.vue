<script setup>
import { ref, onMounted, provide } from 'vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import SkillsSection from './components/SkillsSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import ContactSection from './components/ContactSection.vue'
import NavBar from './components/NavBar.vue'
import ScrollTop from './components/ScrollTop.vue'
import StardustBackground from './components/StardustBackground.vue'
import SmokeTrail from './components/SmokeTrail.vue'
import translations from './data/translations.js'

const isLoaded = ref(false)

const locale = ref(localStorage.getItem('locale') || 'en')

const setLocale = (newLocale) => {
  locale.value = newLocale
  localStorage.setItem('locale', newLocale)
  document.documentElement.lang = newLocale
}

const t = (key) => {
  const currentDict = translations[locale.value] || translations['en']
  return currentDict[key] || translations['en'][key] || key
}

provide('locale', locale)
provide('setLocale', setLocale)
provide('t', t)

onMounted(() => {
  document.documentElement.lang = locale.value
  setTimeout(() => {
    isLoaded.value = true
  }, 100)
})
</script>

<template>
  <div class="portfolio" :class="{ loaded: isLoaded }">
    <StardustBackground />
    <SmokeTrail />
    <NavBar />
    <main>
      <HeroSection />
      <AboutSection />
      <SkillsSection />
      <ProjectsSection />
      <ContactSection />
    </main>
    <footer class="footer">
      <p>&copy; 2026 Sana. {{ t('footer_rights') }}</p>
    </footer>
    <ScrollTop />
  </div>
</template>

<style scoped>
.portfolio {
  opacity: 0;
  transition: opacity 0.5s ease;
}

.portfolio.loaded {
  opacity: 1;
}

main {
  overflow-x: hidden;
}

.footer {
  text-align: center;
  padding: 2rem;
  background: var(--nav-bg);
  color: var(--subtext-color);
  font-size: 0.9rem;
}
</style>
