<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import siteInfo from '@/data/siteInfo.js'
import projectsData from '@/data/projects.js'

const isVisible = ref(false)
const sectionRef = ref(null)

// start counters at 0 so they animate up when visible
const projects = ref(0)
const clients = ref(0)
const satisfaction = ref(0)
const yearsExp = ref(siteInfo.yearsExperience)

let observer = null

const handleIntersection = (entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      isVisible.value = true
      startCountUp()
    }
  })
}

function animateCount(target, refTarget, duration = 800) {
  const start = 0
  const end = Number(target)
  if (end === 0) {
    refTarget.value = 0
    return
  }
  const startTime = performance.now()

  function tick(now) {
    const elapsed = now - startTime
    const progress = Math.min(elapsed / duration, 1)
    refTarget.value = Math.floor(progress * (end - start) + start)
    if (progress < 1) {
      requestAnimationFrame(tick)
    } else {
      refTarget.value = end
    }
  }

  requestAnimationFrame(tick)
}

function startCountUp() {
  // prevent restarting if already animated
  if (projects.value > 0) return
  animateCount(projectsData.length, projects)
  animateCount(siteInfo.happyClients, clients)
  // satisfaction is a percentage; animate but keep % when displaying
  animateCount(siteInfo.satisfaction, satisfaction)
}

onMounted(() => {
  observer = new IntersectionObserver(handleIntersection, { threshold: 0.2 })
  if (sectionRef.value) observer.observe(sectionRef.value)
})

onBeforeUnmount(() => {
  if (observer && sectionRef.value) observer.unobserve(sectionRef.value)
})
</script>

<template>
  <section id="about" ref="sectionRef" class="about">
    <div class="container" :class="{ visible: isVisible }">
      <div class="section-header">
        <span class="section-tag">About Me</span>
        <h2 class="section-title">Passionate About Creating</h2>
      </div>

      <div class="about-content">
        <div class="about-image">
          <div class="image-frame">
            <div class="image-placeholder">
              <img src="@/assets/images/sana.png" alt="Sana" class="about-image-img" />
            </div>
            <div class="frame-decoration"></div>
          </div>
          <div class="experience-badge">
            <span class="exp-number">{{ yearsExp }}+</span>
            <span class="exp-text">Years Experience</span>
          </div>
        </div>

        <div class="about-text">
          <p class="lead">
            I'm a passionate web developer based in the digital world, 
            dedicated to creating beautiful, functional, and user-centered web applications.
          </p>
          <p>
            With a strong foundation in modern development technologies, 
            I bring ideas to life through clean code and innovative solutions. I believe that 
            great development is not just about functionality—it's about solving problems and creating 
            meaningful user experiences.
          </p>
          <p>
            When I'm not coding, you'll find me exploring new technologies, contributing to 
            open-source projects, or sharing knowledge with the developer community.
          </p>

          <div class="stats">
            <div class="stat-item">
              <span class="stat-number">{{ projects }}+</span>
              <span class="stat-label">Projects Completed</span>
            </div>
            <div class="stat-item">
              <span class="stat-number">{{ clients }}+</span>
              <span class="stat-label">Happy Clients</span>
            </div>
            <div class="stat-item">
              <span class="stat-number">{{ satisfaction }}%</span>
              <span class="stat-label">Satisfaction</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.about {
  padding: 6rem 2rem;
  background: linear-gradient(180deg, #0f0f1a 0%, #1a1a2e 100%);
  position: relative;
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

.about-content {
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: 4rem;
  align-items: center;
}

.about-image {
  position: relative;
}

.image-frame {
  position: relative;
  border-radius: 20px;
  overflow: hidden;
}

.image-placeholder {
  aspect-ratio: 4/5;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  border: 2px solid rgba(102, 126, 234, 0.2);
  overflow: hidden;
}

.about-image-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.frame-decoration {
  position: absolute;
  inset: -10px;
  border: 2px solid rgba(102, 126, 234, 0.3);
  border-radius: 25px;
  z-index: -1;
}

.experience-badge {
  position: absolute;
  bottom: -20px;
  right: -20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 1.5rem;
  border-radius: 15px;
  text-align: center;
  box-shadow: 0 10px 40px rgba(102, 126, 234, 0.4);
}

.exp-number {
  display: block;
  font-size: 2.5rem;
  font-weight: 800;
  color: #fff;
  line-height: 1;
}

.exp-text {
  display: block;
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.8);
  margin-top: 0.3rem;
}

.about-text {
  color: #a0a0a0;
}

.lead {
  font-size: 1.3rem;
  color: #e0e0e0;
  line-height: 1.8;
  margin-bottom: 1.5rem;
}

.about-text p {
  line-height: 1.8;
  margin-bottom: 1rem;
}

.stats {
  display: flex;
  gap: 2rem;
  margin-top: 2rem;
  padding-top: 2rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.stat-item {
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 2.5rem;
  font-weight: 700;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.stat-label {
  display: block;
  font-size: 0.85rem;
  color: #888;
  margin-top: 0.3rem;
}

@media (max-width: 968px) {
  .about-content {
    grid-template-columns: 1fr;
    gap: 3rem;
  }

  .about-image {
    max-width: 350px;
    margin: 0 auto;
  }

  .section-title {
    font-size: 2rem;
  }

  .stats {
    justify-content: center;
  }

  .experience-badge {
    right: 0;
  }
}
</style>
