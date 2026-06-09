<script setup>
import { ref, onMounted, onBeforeUnmount, inject } from 'vue'
import siteInfo from '@/data/siteInfo.js'
import projectsData from '@/data/projects.js'

const t = inject('t')
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
        <span class="section-tag">{{ t('about_tag') }}</span>
        <h2 class="section-title">{{ t('about_title') }}</h2>
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
            <span class="exp-text">{{ t('about_exp') }}</span>
          </div>
        </div>

        <div class="about-text">
          <p class="lead">
            {{ t('about_lead') }}
          </p>
          <p>
            {{ t('about_p1') }}
          </p>
          <p>
            {{ t('about_p2') }}
          </p>

          <div class="stats">
            <div class="stat-item">
              <span class="stat-number">{{ projects }}+</span>
              <span class="stat-label">{{ t('about_stat_completed') }}</span>
            </div>
            <div class="stat-item">
              <span class="stat-number">{{ clients }}+</span>
              <span class="stat-label">{{ t('about_stat_clients') }}</span>
            </div>
            <div class="stat-item">
              <span class="stat-number">{{ satisfaction }}%</span>
              <span class="stat-label">{{ t('about_stat_satisfaction') }}</span>
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
  background: transparent;
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
  color: var(--heading-color);
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
  background: var(--card-bg);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  border: 2px solid var(--card-border);
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
  color: var(--subtext-color);
}

.lead {
  font-size: 1.3rem;
  color: var(--text-color);
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
  border-top: 1px solid var(--card-border);
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
  color: var(--subtext-color);
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
