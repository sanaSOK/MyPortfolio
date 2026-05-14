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
          <span class="title-word highlight">Web Developer</span>
        </h2>
        <p class="description">
          I craft beautiful digital experiences with clean code and modern technologies. 
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
          <div class="avatar-blob">
            <img 
              src="@/assets/images/me.png" 
              alt="SOK Sana" 
              class="profile-image"
            />
          </div>
          <div class="decorative-dot dot-1"></div>
          <div class="decorative-dot dot-2"></div>
          <div class="decorative-dot dot-3"></div>
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
  width: 300px;
  height: 300px;
}

.avatar-blob {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.profile-image {
  width: 280px;
  height: 280px;
  object-fit: cover;
  border-radius: 40% 60% 70% 30% / 40% 50% 60% 50%;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1), border-radius 0.6s ease-in-out;
  animation: blob-morph 6s ease-in-out infinite, photo-fly 1.2s cubic-bezier(0.34, 1.56, 0.64, 1) both;
  box-shadow: 0 30px 80px rgba(102, 126, 234, 0.3);
}

.avatar-blob:hover .profile-image {
  transform: scale(1.08) rotate(2deg);
  border-radius: 40% 60% 60% 40% / 40% 40% 60% 60%;
}

@keyframes blob-morph {
  0%, 100% {
    border-radius: 40% 60% 70% 30% / 40% 50% 60% 50%;
  }
  25% {
    border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
  }
  50% {
    border-radius: 70% 30% 60% 40% / 30% 60% 40% 70%;
  }
  75% {
    border-radius: 30% 70% 40% 60% / 70% 40% 60% 30%;
  }
}

.decorative-dot {
  position: absolute;
  border-radius: 50%;
  animation: float 4s ease-in-out infinite;
}

.dot-1 {
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  bottom: -20px;
  left: -30px;
  animation-delay: 0s;
}

.dot-2 {
  width: 70px;
  height: 70px;
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  top: 20px;
  right: -40px;
  animation-delay: -1s;
}

.dot-3 {
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  bottom: 40px;
  right: -25px;
  animation-delay: -2s;
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
