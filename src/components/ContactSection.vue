<script setup>
import { ref, onMounted } from 'vue'

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
import IconLinkedIn from './icons/IconLinkedIn.vue'
import IconTwitter from './icons/IconTwitter.vue'
import IconDribbble from './icons/IconDribbble.vue'

const socialLinks = [
  { icon: IconGitHub, name: 'GitHub', url: 'https://github.com/sanaSOK' },
  { icon: IconLinkedIn, name: 'LinkedIn', url: '#' },
  { icon: IconTwitter, name: 'Twitter', url: '#' },
  { icon: IconDribbble, name: 'Dribbble', url: '#' }
]

const contactInfo = [
  { icon: IconEmail, label: 'Email', value: 'johndoe@email.com' },
  { icon: IconPhone, label: 'Phone', value: '+1 234 567 890' },
  { icon: IconLocation, label: 'Location', value: 'New York, USA' }
]

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
        <span class="section-tag">Contact</span>
        <h2 class="section-title">Let's Work Together</h2>
        <p class="section-subtitle">
          Have a project in mind? Let's create something amazing together.
        </p>
      </div>

      <div class="contact-content">
        <div class="contact-info">
          <h3 class="info-title">Get In Touch</h3>
          <p class="info-text">
            I'm always open to discussing new projects, creative ideas, 
            or opportunities to be part of your visions.
          </p>

          <div class="info-items">
            <div v-for="info in contactInfo" :key="info.label" class="info-item">
              <span class="info-icon"><component :is="info.icon" /></span>
              <div>
                <span class="info-label">{{ info.label }}</span>
                <span class="info-value">{{ info.value }}</span>
              </div>
            </div>
          </div>

          <div class="social-links">
            <h4>Follow Me</h4>
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
              <label for="name">Your Name</label>
              <input 
                type="text" 
                id="name" 
                v-model="form.name"
                placeholder="SOK Sana"
                required
              />
            </div>
            <div class="form-group">
              <label for="email">Your Email</label>
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
            <label for="subject">Subject</label>
            <input 
              type="text" 
              id="subject" 
              v-model="form.subject"
              placeholder="Project Inquiry"
              required
            />
          </div>
          <div class="form-group">
            <label for="message">Message</label>
            <textarea 
              id="message" 
              v-model="form.message"
              placeholder="Tell me about your project..."
              rows="5"
              required
            ></textarea>
          </div>
          <button type="submit" class="submit-btn" :disabled="isSubmitting">
            <span v-if="isSubmitting">Sending...</span>
            <span v-else-if="submitSuccess">✓ Message Sent!</span>
            <span v-else>Send Message →</span>
          </button>
        </form>
      </div>
    </div>
  </section>
</template>

<style scoped>
.contact {
  padding: 6rem 2rem;
  background: linear-gradient(180deg, #1a1a2e 0%, #0f0f1a 100%);
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
  margin: 0 0 1rem;
}

.section-subtitle {
  font-size: 1.1rem;
  color: #888;
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
  color: #fff;
  margin: 0 0 1rem;
}

.info-text {
  color: #888;
  line-height: 1.8;
  margin-bottom: 2rem;
}

.info-items {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.info-icon {
  width: 72px;
  height: 72px;
  background: radial-gradient(circle at 30% 30%, rgba(102,126,234,0.95), rgba(118,75,162,0.9));
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.6rem;
  color: #fff;
  box-shadow: 0 10px 30px rgba(118,75,162,0.2);
}
.info-icon svg {
  width: 30px;
  height: 30px;
  color: #fff;
}

.info-label {
  display: block;
  font-size: 0.85rem;
  color: #666;
}

.info-value {
  display: block;
  color: #e0e0e0;
  font-weight: 500;
}

.social-links h4 {
  color: #fff;
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
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.05);
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
  color: #e0e0e0;
  font-size: 0.9rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 1rem 1.2rem;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  color: #fff;
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
    align-items: center;
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
