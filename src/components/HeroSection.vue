<script setup>
import { ref, onMounted } from 'vue'

const isVisible = ref(false)

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true
  }, 200)
})

const scrollToContact = () => {
  const element = document.getElementById('contact')
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}
</script>

<template>
  <section id="hero" class="hero">
    <div class="hero-bg">
      <div class="gradient-orb orb-1"></div>
      <div class="gradient-orb orb-2"></div>
      <div class="gradient-orb orb-3"></div>
    </div>
    
    <div class="hero-content" :class="{ visible: isVisible }">
      <div class="hero-text">
        <span class="greeting">Hello, I'm</span>
        <h1 class="name">
          <span class="name-inner">
            <span class="name-part sok">SOK</span>
            <span class="name-part spacer">&nbsp;</span>
            <span class="name-part sana">Sana</span>
          </span>
        </h1>
        <h2 class="title">
          <span class="title-word">Creative</span>
          <span class="title-word highlight">Developer</span>
          <span class="title-word">&amp; Designer</span>
        </h2>
        <p class="description">
          I craft beautiful digital experiences with clean code and stunning design. 
          Passionate about creating innovative solutions that make a difference.
        </p>
        <div class="hero-buttons">
          <button class="btn btn-primary" @click="scrollToContact">
            Get In Touch
            <span class="btn-icon">→</span>
          </button>
          <a href="#projects" class="btn btn-secondary">
            View My Work
          </a>
        </div>
      </div>
      
      <div class="hero-visual">
        <div class="avatar-container">
          <div class="avatar-ring"></div>
          
          <div class="avatar">
            <!-- Replace with your profile image -->
            <img 
              src="@/assets/images/QR.png" 
              alt="SOK Sana" 
              class="profile-image"
            />
          </div>
          
          
          
        </div>
      </div>
    </div>

    <div class="scroll-indicator">
      <span>Scroll Down</span>
      <div class="scroll-arrow"></div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  padding: 6rem 2rem 2rem;
}

.hero-bg {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.gradient-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.5;
  animation: float 8s ease-in-out infinite;
}

.orb-1 {
  width: 400px;
  height: 400px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  top: -100px;
  right: -100px;
  animation-delay: 0s;
}

.orb-2 {
  width: 300px;
  height: 300px;
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  bottom: -50px;
  left: -50px;
  animation-delay: -2s;
}

.orb-3 {
  width: 250px;
  height: 250px;
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: -4s;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0) scale(1);
  }
  50% {
    transform: translateY(-30px) scale(1.05);
  }
}

.hero-content {
  max-width: 1200px;
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
  position: relative;
  z-index: 1;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s ease;
}

.hero-content.visible {
  opacity: 1;
  transform: translateY(0);
}

.greeting {
  display: inline-block;
  font-size: 1.2rem;
  color: #667eea;
  font-weight: 500;
  margin-bottom: 0.5rem;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.name {
  font-size: 4.5rem;
  font-weight: 800;
  margin: 0;
    position: relative;
    line-height: 1.05;
}

.name-inner {
  display: inline-block;
  position: relative;
  transform-origin: left center;
  animation: name-entrance 900ms cubic-bezier(.2,.9,.2,1) both;
}

.name-part {
  display: inline-block;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.name-part.sok {
  background-image: linear-gradient(90deg, #ffffff 0%, #f0f0f0 20%, #bdb6ff 50%, #8a78ff 100%);
  font-weight: 900;
  filter: drop-shadow(0 6px 18px rgba(102,86,234,0.18));
  transform: translateY(0);
}

.name-part.sana {
  background-image: linear-gradient(90deg, #d6d6ff 0%, #8a78ff 40%, #667eea 100%);
  font-weight: 800;
  transform: translateY(0);
}

/* animated gradient sweep */
.name-part.sok, .name-part.sana {
  background-size: 200% 100%;
  animation: gradient-sweep 3.5s ease-in-out infinite;
}

@keyframes gradient-sweep {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

@keyframes name-entrance {
  0% { opacity: 0; transform: translateY(18px) scale(0.98); }
  100% { opacity: 1; transform: translateY(0) scale(1); }
}

/* underline sweep */
.name-inner::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: -12px;
  height: 8px;
  background: linear-gradient(90deg, rgba(102,126,234,0.18), rgba(118,75,162,0.2));
  border-radius: 10px;
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 700ms cubic-bezier(.2,.9,.2,1);
}

.name-inner:hover::after {
  transform: scaleX(1);
}


.title {
  font-size: 1.8rem;
  font-weight: 400;
  margin: 1rem 0;
  color: #a0a0a0;
}

.title-word {
  display: inline-block;
  margin-right: 0.5rem;
}

.title-word.highlight {
  color: #667eea;
  font-weight: 600;
}

.description {
  font-size: 1.1rem;
  color: #888;
  line-height: 1.8;
  max-width: 500px;
  margin: 1.5rem 0 2rem;
}

.hero-buttons {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.btn {
  padding: 1rem 2rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  text-decoration: none;
  border: none;
}

.btn-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 0 4px 20px rgba(102, 126, 234, 0.4);
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 30px rgba(102, 126, 234, 0.6);
}

.btn-icon {
  transition: transform 0.3s ease;
}

.btn-primary:hover .btn-icon {
  transform: translateX(5px);
}

.btn-secondary {
  background: transparent;
  color: #e0e0e0;
  border: 2px solid rgba(255, 255, 255, 0.2);
}

.btn-secondary:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.4);
}

.hero-visual {
  display: flex;
  justify-content: center;
  align-items: center;
}

.avatar-container {
  position: relative;
  width: 350px;
  height: 350px;
}

.avatar-ring {
  position: absolute;
  inset: 0;
  border-radius: 22px;
  border: 3px solid transparent;
  background: linear-gradient(135deg, #667eea, #764ba2, #f093fb) border-box;
  -webkit-mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
  mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  animation: rotate 10s linear infinite;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.avatar {
  position: absolute;
  inset: 20px;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  border-radius: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
  overflow: hidden;
}

.profile-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 12px;
}

.avatar-emoji {
  font-size: 8rem;
}



.scroll-indicator {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  color: #666;
  font-size: 0.85rem;
  animation: bounce 2s ease-in-out infinite;
}

.scroll-arrow {
  width: 20px;
  height: 20px;
  border-right: 2px solid #666;
  border-bottom: 2px solid #666;
  transform: rotate(45deg);
}

@keyframes bounce {
  0%, 100% {
    transform: translateX(-50%) translateY(0);
  }
  50% {
    transform: translateX(-50%) translateY(10px);
  }
}

@media (max-width: 968px) {
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 3rem;
  }

  .hero-text {
    order: 2;
  }

  .hero-visual {
    order: 1;
  }

  .name {
    font-size: 3rem;
  }

  .title {
    font-size: 1.4rem;
  }

  .description {
    margin-left: auto;
    margin-right: auto;
  }

  .hero-buttons {
    justify-content: center;
  }

  .avatar-container {
    width: 250px;
    height: 250px;
  }

  .avatar-emoji {
    font-size: 5rem;
  }
}
</style>
