<script setup>
import { ref, onMounted, inject, computed } from 'vue'

const t = inject('t')
const isVisible = ref(false)
const sectionRef = ref(null)

const form = ref({
  name: '',
  email: '',
  subject: '',
  message: ''
})

const isSubmitting = ref(false)
const submitSuccess = ref(false)

const handleSubmit = async () => {
  isSubmitting.value = true
  // Simulate API call
  await new Promise(resolve => setTimeout(resolve, 1500))
  isSubmitting.value = false
  submitSuccess.value = true
  form.value = { name: '', email: '', subject: '', message: '' }
  
  setTimeout(() => {
    submitSuccess.value = false
  }, 3000)
}

import IconEmail from './icons/IconEmail.vue'
import IconPhone from './icons/IconPhone.vue'
import IconLocation from './icons/IconLocation.vue'
import IconGitHub from './icons/IconGitHub.vue'
import IconYouTube from './icons/IconYouTube.vue'
import IconFacebook from './icons/IconFacebook.vue'
import IconInstagram from './icons/IconInstagram.vue'

const socialLinks = [
  { icon: IconGitHub, name: 'GitHub', url: 'https://github.com/sanaSOK' },
  { icon: IconYouTube, name: 'YouTube', url: 'https://www.youtube.com/' },
  { icon: IconFacebook, name: 'Facebook', url: 'https://www.facebook.com/' },
  { icon: IconInstagram, name: 'Instagram', url: 'https://www.instagram.com/' }
]

const contactInfo = computed(() => [
  { icon: IconEmail, label: t('contact_info_email'), value: 'sanasokitc2023@gmail.com', labelKey: 'contact_info_email' },
  { icon: IconPhone, label: t('contact_info_phone'), value: '(+885) 66 737 549', labelKey: 'contact_info_phone' },
  { icon: IconLocation, label: t('contact_info_loc'), value: 'Phnom Penh, Cambodia', labelKey: 'contact_info_loc' }
])

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
  <section id="contact" ref="sectionRef" class="contact">
    <div class="container" :class="{ visible: isVisible }">
      <div class="section-header">
        <span class="section-tag">{{ t('contact_tag') }}</span>
        <h2 class="section-title">{{ t('contact_title') }}</h2>
        <p class="section-subtitle">
          {{ t('contact_subtitle') }}
        </p>
      </div>

      <div class="contact-content">
        <div class="contact-info">
          <h3 class="info-title">{{ t('contact_info_title') }}</h3>
          <p class="info-text">
            {{ t('contact_info_text') }}
          </p>

          <div class="info-items">
            <div v-for="info in contactInfo" :key="info.label" class="info-card">
              <div class="info-icon-wrapper">
                <span class="info-icon"><component :is="info.icon" /></span>
              </div>
              <div class="info-text-wrapper">
                <span class="info-label">{{ info.label }}</span>
                <a v-if="info.labelKey === 'contact_info_email'" :href="'mailto:' + info.value" class="info-value link">{{ info.value }}</a>
                <a v-else-if="info.labelKey === 'contact_info_phone'" :href="'tel:' + info.value.replace(/\s+/g, '')" class="info-value link">{{ info.value }}</a>
                <span v-else class="info-value">{{ info.value }}</span>
              </div>
            </div>
          </div>

          <div class="social-links">
            <h4>{{ t('contact_follow') }}</h4>
            <div class="social-icons">
              <a v-for="social in socialLinks" :key="social.name" :href="social.url" class="social-link" :title="social.name">
                <component :is="social.icon" />
              </a>
            </div>
          </div>
        </div>

        <form class="contact-form" @submit.prevent="handleSubmit">
          <div class="form-row">
            <div class="form-group">
              <label for="name">{{ t('contact_form_name') }}</label>
              <input 
                type="text" 
                id="name" 
                v-model="form.name"
                placeholder="SOK Sana"
                required
              />
            </div>
            <div class="form-group">
              <label for="email">{{ t('contact_form_email') }}</label>
              <input 
                type="email" 
                id="email" 
                v-model="form.email"
                placeholder="sana@gmail.com"
                required
              />
            </div>
          </div>
          <div class="form-group">
            <label for="subject">{{ t('contact_form_subject') }}</label>
            <input 
              type="text" 
              id="subject" 
              v-model="form.subject"
              placeholder="Project Inquiry"
              required
            />
          </div>
          <div class="form-group">
            <label for="message">{{ t('contact_form_msg') }}</label>
            <textarea 
              id="message" 
              v-model="form.message"
              :placeholder="t('contact_form_msg_placeholder')"
              rows="5"
              required
            ></textarea>
          </div>
          <button type="submit" class="submit-btn" :disabled="isSubmitting">
            <span v-if="isSubmitting">{{ t('contact_form_sending') }}</span>
            <span v-else-if="submitSuccess">✓ {{ t('contact_form_sent') }}</span>
            <span v-else>{{ t('contact_form_send') }} →</span>
          </button>
        </form>
      </div>
    </div>
  </section>
</template>

<style scoped>
.contact {
  padding: 6rem 2rem;
  background: transparent;
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
  margin: 0 0 1rem;
}

.section-subtitle {
  font-size: 1.1rem;
  color: var(--subtext-color);
  max-width: 500px;
  margin: 0 auto;
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 4rem;
}

.contact-info {
  padding-right: 2rem;
}

.info-title {
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--heading-color);
  margin: 0 0 1rem;
}

.info-text {
  color: var(--subtext-color);
  line-height: 1.8;
  margin-bottom: 2rem;
}

.info-items {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 2.5rem;
  width: 100%;
}

.info-card {
  display: flex;
  align-items: center;
  gap: 1.25rem;
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  padding: 1.2rem 1.5rem;
  border-radius: 16px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  width: 100%;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
}

.info-card:hover {
  transform: translateY(-3px) scale(1.01);
  border-color: rgba(102, 126, 234, 0.4);
  background: rgba(102, 126, 234, 0.04);
  box-shadow: 0 10px 30px rgba(102, 126, 234, 0.1);
}

.info-icon-wrapper {
  flex-shrink: 0;
}

.info-icon {
  width: 54px;
  height: 54px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.25);
  transition: transform 0.3s ease;
}

.info-card:hover .info-icon {
  transform: scale(1.08) rotate(-5deg);
}

.info-icon svg {
  width: 24px;
  height: 24px;
  color: #fff;
}

.info-text-wrapper {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
  text-align: left;
}

.info-label {
  font-size: 0.8rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  color: var(--subtext-color);
  opacity: 0.7;
}

.info-value {
  font-size: 1.05rem;
  color: var(--text-color);
  font-weight: 600;
  transition: color 0.2s ease;
  word-break: break-all;
}

.info-value.link {
  text-decoration: none;
}

.info-value.link:hover {
  color: #667eea;
}

.social-links h4 {
  color: var(--heading-color);
  font-size: 1rem;
  margin: 0 0 1rem;
}

.social-icons {
  display: flex;
  gap: 0.75rem;
}

.social-link {
  width: 54px;
  height: 54px;
  background: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(0,0,0,0.15));
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  text-decoration: none;
  transition: transform 0.25s ease, box-shadow 0.25s ease;
}
.social-link svg {
  width: 20px;
  height: 20px;
  color: #ddd;
}

.social-link:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(0,0,0,0.6);
}

.contact-form {
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 20px;
  padding: 2.5rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  color: var(--text-color);
  font-size: 0.9rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 1rem 1.2rem;
  background: var(--input-bg);
  border: 1px solid var(--input-border);
  border-radius: 12px;
  color: var(--heading-color);
  font-size: 1rem;
  font-family: inherit;
  transition: all 0.3s ease;
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: #555;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: rgba(102, 126, 234, 0.5);
  background: rgba(102, 126, 234, 0.05);
}

.form-group textarea {
  resize: vertical;
  min-height: 120px;
}

.submit-btn {
  width: 100%;
  padding: 1.2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border: none;
  border-radius: 12px;
  color: #fff;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-btn:hover:not(:disabled) {
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
}

.submit-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

@media (max-width: 968px) {
  .contact-content {
    grid-template-columns: 1fr;
    gap: 3rem;
  }

  .contact-info {
    padding-right: 0;
    text-align: center;
  }

  .info-items {
    margin-left: auto;
    margin-right: auto;
    max-width: 450px;
  }

  .social-icons {
    justify-content: center;
  }
}

@media (max-width: 600px) {
  .section-title {
    font-size: 2rem;
  }

  .form-row {
    grid-template-columns: 1fr;
  }

  .contact-form {
    padding: 1.5rem;
  }
}
</style>
