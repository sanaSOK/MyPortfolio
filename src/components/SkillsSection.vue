<script setup>
import { ref, onMounted } from 'vue'

const isVisible = ref(false)
const sectionRef = ref(null)

const skills = [
  { name: 'HTML/CSS', level: 95, icon: '🎨', color: '#e34c26' },
  { name: 'JavaScript', level: 90, icon: '⚡', color: '#f7df1e' },
  { name: 'Vue.js', level: 88, icon: '💚', color: '#42b883' },
  { name: 'React', level: 85, icon: '⚛️', color: '#61dafb' },
  { name: 'Node.js', level: 82, icon: '🟢', color: '#339933' },
  { name: 'Python', level: 80, icon: '🐍', color: '#3776ab' },
  { name: 'Git', level: 85, icon: '📦', color: '#f05032' },
]

const services = [
  {
    icon: '💻',
    title: 'Web Development',
    description: 'Building responsive and performant web applications with modern technologies.',
    fullDescription: 'I build full-stack web applications using modern frameworks, optimized for accessibility and performance. I handle front-end UI, state management, backend APIs, and deployment.',
    image: '💻',
    examples: [
      { title: 'E-Commerce E-Market', link: 'https://emarket-three.vercel.app' },
      { title: 'Automatic Proccessing', link: 'https://automaton-nu.vercel.app' }
    ]
  },
  {
    icon: '📱',
    title: 'Mobile Apps',
    description: 'Developing cross-platform mobile applications for iOS and Android.'
  },
  {
    icon: '🚀',
    title: 'Performance Optimization',
    description: 'Optimizing applications for speed and the best user experience.'
  }
]

import SkillDetail from './SkillDetail.vue'


const showSkillModal = ref(false)
const selectedSkill = ref(null)

function openSkill(service) {
  selectedSkill.value = service
  showSkillModal.value = true
}

function closeSkill() {
  showSkillModal.value = false
  selectedSkill.value = null
}

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        isVisible.value = true
      }
    })
  }, { threshold: 0.2 })
  
  if (sectionRef.value) {
    observer.observe(sectionRef.value)
  }
})
</script>

<template>
  <section id="skills" ref="sectionRef" class="skills">
    <div class="container" :class="{ visible: isVisible }">
      <div class="section-header">
        <span class="section-tag">My Skills</span>
        <h2 class="section-title">What I Do Best</h2>
      </div>

      <div class="services-grid">
        <div v-for="(service, index) in services" :key="index" class="service-card" :style="{ '--delay': index * 0.1 + 's' }" @click="openSkill(service)">
          <span class="service-icon">{{ service.icon }}</span>
          <h3 class="service-title">{{ service.title }}</h3>
          <p class="service-description">{{ service.description }}</p>
        </div>
      </div>

      <SkillDetail v-if="showSkillModal" :skill="selectedSkill" @close="closeSkill" />

      <div class="skills-container">
        <h3 class="skills-heading">Technical Proficiency</h3>
        <div class="skills-grid">
          <div v-for="(skill, index) in skills" :key="index" class="skill-item" :style="{ '--delay': index * 0.1 + 's' }">
            <div class="skill-header">
              <span class="skill-icon">{{ skill.icon }}</span>
              <span class="skill-name">{{ skill.name }}</span>
              <span class="skill-percent">{{ skill.level }}%</span>
            </div>
            <div class="skill-bar">
              <div 
                class="skill-progress" 
                :style="{ 
                  '--progress': skill.level + '%',
                  '--color': skill.color
                }"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.skills {
  padding: 6rem 2rem;
  background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  opacity: 0;
  transform: translateY(50px);
  transition: all 0.8s ease;
}

.container.visible {
  opacity: 1;
  transform: translateY(0);
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
}

.section-tag {
  display: inline-block;
  padding: 0.5rem 1.5rem;
  background: rgba(102, 126, 234, 0.1);
  border: 1px solid rgba(102, 126, 234, 0.3);
  border-radius: 50px;
  color: #667eea;
  font-size: 0.9rem;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-bottom: 1rem;
}

.section-title {
  font-size: 2.8rem;
  font-weight: 700;
  color: #fff;
  margin: 0;
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  margin-bottom: 4rem;
}

.service-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 20px;
  padding: 2rem;
  text-align: center;
  transition: all 0.3s ease;
  animation: fadeInUp 0.6s ease forwards;
  animation-delay: var(--delay);
  opacity: 0;
}

.container.visible .service-card {
  opacity: 1;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.service-card:hover {
  background: rgba(102, 126, 234, 0.1);
  border-color: rgba(102, 126, 234, 0.3);
  transform: translateY(-10px);
}

.service-icon {
  font-size: 3rem;
  display: block;
  margin-bottom: 1rem;
}

.service-title {
  font-size: 1.3rem;
  font-weight: 600;
  color: #fff;
  margin: 0 0 0.5rem;
}

.service-description {
  font-size: 0.95rem;
  color: #888;
  margin: 0;
  line-height: 1.6;
}

.skills-container {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 20px;
  padding: 3rem;
}

.skills-heading {
  font-size: 1.5rem;
  font-weight: 600;
  color: #fff;
  margin: 0 0 2rem;
  text-align: center;
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}

.skill-item {
  opacity: 0;
  animation: fadeInUp 0.6s ease forwards;
  animation-delay: var(--delay);
}

.container.visible .skill-item {
  opacity: 1;
}

.skill-header {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.5rem;
}

.skill-icon {
  font-size: 1.2rem;
}

.skill-name {
  font-weight: 500;
  color: #e0e0e0;
  flex: 1;
}

.skill-percent {
  font-weight: 600;
  color: #667eea;
}

.skill-bar {
  height: 8px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  overflow: hidden;
}

.skill-progress {
  height: 100%;
  width: 0;
  background: linear-gradient(90deg, var(--color), #667eea);
  border-radius: 10px;
  transition: width 1s ease;
}

.container.visible .skill-progress {
  width: var(--progress);
}

@media (max-width: 768px) {
  .section-title {
    font-size: 2rem;
  }

  .skills-container {
    padding: 1.5rem;
  }

  .skills-grid {
    grid-template-columns: 1fr;
  }
}
</style>
