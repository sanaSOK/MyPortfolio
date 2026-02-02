<script setup>
import { ref, onMounted, computed } from 'vue'

const isVisible = ref(false)
const sectionRef = ref(null)
const activeFilter = ref('all')

const projects = [
  {
    id: 1,
    title: 'E-Commerce E-Market',
    category: 'web',
    description: 'A full-featured online store with cart, payments, and admin dashboard.',
    image: '🛒',
    tags: ['Vue.js', 'Node.js', 'MongoDB'],
    link: 'https://emarket-three.vercel.app',
    github: 'https://github.com/sanaSOK'
  },
  {
    id: 2,
    title: 'Coffee Management ',
    category: 'app',
    description: 'Productivity app with drag-and-drop, reminders, and team collaboration.',
    image: '☕',
    tags: ['React', 'Firebase', 'Tailwind'],
    link: 'https://web-design-finale-project.vercel.app',
    github: 'https://github.com/sanaSOK'
  },
  {
    id: 3,
    title: 'Portfolio Website',
    category: 'design',
    description: 'Modern portfolio design with smooth animations and dark theme.',
    image: '🎨',
    tags: ['Vue.js', 'CSS3', 'Animation'],
    link: 'https://portfolio-web-rho-amber.vercel.app',
    github: 'https://github.com/sanaSOK'
  },
  {
    id: 4,
    title: 'Automatic Proccessing',
    category: 'web',
    description: 'Real-time weather app with forecasts, maps, and location tracking.',
    image: '🎯',
    tags: ['JavaScript', 'API', 'Charts'],
    link: 'https://automaton-nu.vercel.app',
    github: 'https://github.com/sanaSOK'
  },
  {
    id: 5,
    title: 'Calculator App',
    category: 'app',
    description: 'Feature-rich calculator app with advanced functions and history tracking.',
    image: '🖩',
    tags: ['React Native', 'GraphQL', 'AWS'],
    link: 'https://calculator-bay-nine-85.vercel.app',
    github: 'https://github.com/sanaSOK'
  },
  {
    id: 6,
    title: 'Online Learning Platform',
    category: 'web',
    description: 'Comprehensive online learning platform with courses, quizzes, and progress tracking.',
    image: '🎓',
    tags: ['Figma', 'Illustrator', 'Branding'],
    link: 'https://online-course-swart-two.vercel.app',
    github: 'https://github.com/sanaSOK'
  }
]

const filters = ['all', 'web', 'app', 'design']

const filteredProjects = computed(() => {
  if (activeFilter.value === 'all') return projects
  return projects.filter(p => p.category === activeFilter.value)
})

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        isVisible.value = true
      }
    })
  }, { threshold: 0.1 })
  
  if (sectionRef.value) {
    observer.observe(sectionRef.value)
  }
})
</script>

<template>
  <section id="projects" ref="sectionRef" class="projects">
    <div class="container" :class="{ visible: isVisible }">
      <div class="section-header">
        <span class="section-tag">My Work</span>
        <h2 class="section-title">Featured Projects</h2>
      </div>

      <div class="filters">
        <button
          v-for="filter in filters"
          :key="filter"
          class="filter-btn"
          :class="{ active: activeFilter === filter }"
          @click="activeFilter = filter"
        >
          {{ filter.charAt(0).toUpperCase() + filter.slice(1) }}
        </button>
      </div>

      <div class="projects-grid">
        <TransitionGroup name="project">
          <div 
            v-for="(project, index) in filteredProjects" 
            :key="project.id" 
            class="project-card"
            :style="{ '--delay': index * 0.1 + 's' }"
          >
            <div class="project-image">
              <span class="project-emoji">{{ project.image }}</span>
              <div class="project-overlay">
                <a :href="project.link" target="_blank" rel="noopener noreferrer" class="project-link" title="View Project">👁️</a>
                <a :href="project.github" target="_blank" rel="noopener noreferrer" class="project-link github" title="View Code on GitHub">
                  <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
                    <path d="M12 .5C5.73.5.5 5.73.5 12c0 5.08 3.29 9.38 7.86 10.9.58.11.79-.25.79-.56 0-.28-.01-1.02-.02-2-3.2.7-3.88-1.54-3.88-1.54-.52-1.33-1.27-1.68-1.27-1.68-1.04-.71.08-.7.08-.7 1.15.08 1.75 1.18 1.75 1.18 1.02 1.75 2.67 1.24 3.32.95.1-.74.4-1.24.73-1.52-2.55-.29-5.24-1.28-5.24-5.69 0-1.26.45-2.28 1.18-3.09-.12-.29-.51-1.46.11-3.05 0 0 .97-.31 3.18 1.18.92-.26 1.9-.39 2.88-.39.98 0 1.96.13 2.88.39 2.21-1.49 3.18-1.18 3.18-1.18.62 1.59.23 2.76.11 3.05.73.81 1.18 1.83 1.18 3.09 0 4.42-2.7 5.4-5.27 5.68.41.35.77 1.04.77 2.1 0 1.52-.01 2.75-.01 3.12 0 .31.21.68.8.56C20.71 21.38 24 17.08 24 12 24 5.73 18.27.5 12 .5z"/>
                  </svg>
                </a>
              </div>
            </div>
            <div class="project-content">
              <span class="project-category">{{ project.category }}</span>
              <h3 class="project-title">{{ project.title }}</h3>
              <p class="project-description">{{ project.description }}</p>
              <div class="project-tags">
                <span v-for="tag in project.tags" :key="tag" class="tag">{{ tag }}</span>
              </div>
            </div>
          </div>
        </TransitionGroup>
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects {
  padding: 6rem 2rem;
  background: linear-gradient(180deg, #16213e 0%, #1a1a2e 100%);
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
  margin-bottom: 3rem;
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

.filters {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 3rem;
  flex-wrap: wrap;
}

.filter-btn {
  padding: 0.7rem 1.5rem;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 50px;
  color: #888;
  font-size: 0.95rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.filter-btn:hover {
  border-color: rgba(102, 126, 234, 0.5);
  color: #e0e0e0;
}

.filter-btn.active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-color: transparent;
  color: #fff;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2rem;
}

.project-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 20px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.project-card:hover {
  transform: translateY(-10px);
  border-color: rgba(102, 126, 234, 0.3);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}

.project-image {
  position: relative;
  height: 220px;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.project-emoji {
  font-size: 5rem;
  transition: transform 0.3s ease;
}

.project-card:hover .project-emoji {
  transform: scale(1.1);
}

.project-overlay {
  position: absolute;
  inset: 0;
  background: rgba(102, 126, 234, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.project-card:hover .project-overlay {
  opacity: 1;
}

.project-link {
  width: 50px;
  height: 50px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  text-decoration: none;
  transition: all 0.3s ease;
}

.project-link:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.1);
}

.project-content {
  padding: 1.5rem;
}

.project-category {
  display: inline-block;
  padding: 0.3rem 0.8rem;
  background: rgba(102, 126, 234, 0.1);
  border-radius: 20px;
  font-size: 0.75rem;
  color: #667eea;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 0.8rem;
}

.project-title {
  font-size: 1.3rem;
  font-weight: 600;
  color: #fff;
  margin: 0 0 0.5rem;
}

.project-description {
  font-size: 0.95rem;
  color: #888;
  line-height: 1.6;
  margin: 0 0 1rem;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.tag {
  padding: 0.3rem 0.7rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 15px;
  font-size: 0.8rem;
  color: #a0a0a0;
}

/* Transitions */
.project-enter-active,
.project-leave-active {
  transition: all 0.5s ease;
}

.project-enter-from,
.project-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

@media (max-width: 768px) {
  .section-title {
    font-size: 2rem;
  }

  .projects-grid {
    grid-template-columns: 1fr;
  }
}
</style>
