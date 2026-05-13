<script setup>
import { ref, onMounted } from 'vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import SkillsSection from './components/SkillsSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import ContactSection from './components/ContactSection.vue'
import NavBar from './components/NavBar.vue'
import ScrollTop from './components/ScrollTop.vue'

const isLoaded = ref(false)

onMounted(() => {
  setTimeout(() => {
    isLoaded.value = true
  }, 100)
})
</script>

<template>
  <div class="portfolio" :class="{ loaded: isLoaded }">
    <NavBar />
    <main>
      <HeroSection />
      <AboutSection />
      <SkillsSection />
      <ProjectsSection />
      <ContactSection />
    </main>
    <footer class="footer">
      <p>&copy; 2026 Sana. All rights reserved.</p>
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
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  color: #a0a0a0;
  font-size: 0.9rem;
}
</style>
