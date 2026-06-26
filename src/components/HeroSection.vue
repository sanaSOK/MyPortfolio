<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isVisible = ref(false)
const nameRef = ref(null)
const gradientAngle = ref(135)

// Split name into individual characters for animation
const sokChars = 'SOK'.split('')
const sanaChars = 'Sana'.split('')

const onMouseMove = (e) => {
  if (!nameRef.value) return
  const rect = nameRef.value.getBoundingClientRect()
  const cx = rect.left + rect.width / 2
  const dx = (e.clientX - cx) / (rect.width / 2)
  gradientAngle.value = 135 + dx * 45
}

const onMouseLeave = () => {
  gradientAngle.value = 135
}

onMounted(() => {
  setTimeout(() => { isVisible.value = true }, 200)
  window.addEventListener('mousemove', onMouseMove)
})

onUnmounted(() => {
  window.removeEventListener('mousemove', onMouseMove)
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

        <!-- Animated name -->
        <h1 class="name">
          <div
            ref="nameRef"
            class="name-3d-wrap"
            @mouseleave="onMouseLeave"
          >
            <!-- SOK -->
            <span class="name-word">
              <span
                v-for="(char, i) in sokChars"
                :key="'sok-' + i"
                class="char"
                :class="'char-' + i"
                :style="{ '--char-index': i, '--gradient-angle': gradientAngle + 'deg' }"
              >{{ char }}</span>
            </span>

            <span class="name-spacer">&nbsp;</span>

            <!-- Sana -->
            <span class="name-word">
              <span
                v-for="(char, i) in sanaChars"
                :key="'sana-' + i"
                class="char char-sana"
                :class="'char-s' + i"
                :style="{ '--char-index': i + 3, '--gradient-angle': gradientAngle + 'deg' }"
              >{{ char }}</span>
            </span>

            <!-- Glow underline -->
            <div class="name-underline"></div>
          </div>
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
/* ─── Base ───────────────────────────────────────────── */
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
  width: 400px; height: 400px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  top: -100px; right: -100px;
  animation-delay: 0s;
}
.orb-2 {
  width: 300px; height: 300px;
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  bottom: -50px; left: -50px;
  animation-delay: -2s;
}
.orb-3 {
  width: 250px; height: 250px;
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: -4s;
}

@keyframes float {
  0%, 100% { transform: translateY(0) scale(1); }
  50%       { transform: translateY(-30px) scale(1.05); }
}

/* ─── Layout ─────────────────────────────────────────── */
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

/* ─── 3D Name Wrapper ────────────────────────────────── */
.name {
  font-size: 4.5rem;
  font-weight: 800;
  margin: 0;
  line-height: 1.1;
}

.name-3d-wrap {
  display: inline-flex;
  align-items: baseline;
  flex-wrap: wrap;
  gap: 0;
  cursor: default;
  position: relative;
  padding-bottom: 14px;
}

.name-word {
  display: inline-flex;
}

.name-spacer {
  display: inline-block;
  width: 0.35em;
}

/* ─── Individual Characters ──────────────────────────── */
.char {
  display: inline-block;
  font-weight: 900;
  position: relative;
  cursor: pointer;
  /* dynamic gradient via CSS variable */
  background-image: linear-gradient(
    var(--gradient-angle, 135deg),
    #ffffff    0%,
    #c4b5fd   20%,
    #818cf8   40%,
    #a78bfa   60%,
    #f0abfc   80%,
    #38bdf8  100%
  );
  background-size: 200% 200%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;

  /* entrance animation */
  animation:
    char-entrance 0.6s cubic-bezier(0.34, 1.56, 0.64, 1) both,
    gradient-shift 4s ease-in-out infinite;
  animation-delay: calc(var(--char-index) * 0.08s), calc(var(--char-index) * 0.3s);

  transition:
    transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1),
    filter 0.2s ease;
}

/* Glowing dot under each char — appears on hover */
.char::before {
  content: '';
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%) scale(0);
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: radial-gradient(circle, #a78bfa, #38bdf8);
  box-shadow: 0 0 8px #a78bfa, 0 0 16px #38bdf8;
  opacity: 0;
  transition: opacity 0.2s ease, transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  pointer-events: none;
}

/* Sana characters get a slightly different gradient hue */
.char.char-sana {
  background-image: linear-gradient(
    var(--gradient-angle, 135deg),
    #e0c3fc   0%,
    #8ec5fc   25%,
    #a78bfa   50%,
    #f9a8d4   75%,
    #67e8f9  100%
  );
  background-size: 200% 200%;
}

/* Hover/touch: characters spread left or right depending on index */
.char:hover {
  filter:
    drop-shadow(0 0 6px rgba(255, 255, 255, 0.9))
    drop-shadow(0 0 14px rgba(139, 92, 246, 1))
    drop-shadow(0 0 30px rgba(56, 189, 248, 0.8))
    brightness(1.5);
  animation-play-state: paused, paused;
}

/* Show dot under char on hover */
.char:hover::before {
  opacity: 1;
  transform: translateX(-50%) scale(1);
}

/* Even index chars slide RIGHT */
.char:nth-child(even):hover {
  transform: translateX(14px) scale(1.3);
}

/* Odd index chars slide LEFT */
.char:nth-child(odd):hover {
  transform: translateX(-14px) scale(1.3);
}

/* ─── Keyframes ──────────────────────────────────────── */
@keyframes char-entrance {
  0% {
    opacity: 0;
    transform: translateX(-30px) scale(0.7);
  }
  100% {
    opacity: 1;
    transform: translateX(0) scale(1);
  }
}

@keyframes gradient-shift {
  0%   { background-position: 0%   50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0%   50%; }
}

/* ─── Animated underline glow ────────────────────────── */
.name-underline {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 4px;
  border-radius: 4px;
  background: linear-gradient(90deg, #667eea, #a78bfa, #38bdf8, #f9a8d4, #667eea);
  background-size: 300% 100%;
  animation: underline-flow 3s linear infinite;
  opacity: 0;
  transform: scaleX(0);
  transform-origin: left center;
  transition: opacity 0.3s ease, transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.name-3d-wrap:hover .name-underline {
  opacity: 1;
  transform: scaleX(1);
}

@keyframes underline-flow {
  0%   { background-position: 0%   50%; }
  100% { background-position: 300% 50%; }
}

/* ─── Title ──────────────────────────────────────────── */
.title {
  font-size: 1.8rem;
  font-weight: 400;
  margin: 1rem 0;
  color: var(--subtext-color);
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
  color: var(--subtext-color);
  line-height: 1.8;
  max-width: 500px;
  margin: 1.5rem 0 2rem;
}

/* ─── Buttons ────────────────────────────────────────── */
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
.btn-icon { transition: transform 0.3s ease; }
.btn-primary:hover .btn-icon { transform: translateX(5px); }

.btn-secondary {
  background: var(--btn-sec-bg);
  color: var(--btn-sec-text);
  border: 2px solid var(--btn-sec-border);
}
.btn-secondary:hover {
  background: var(--btn-sec-hover);
  border-color: var(--btn-sec-text);
}

/* ─── Avatar ─────────────────────────────────────────── */
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
  overflow: visible;
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
  0%, 100% { border-radius: 40% 60% 70% 30% / 40% 50% 60% 50%; }
  25%       { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
  50%       { border-radius: 70% 30% 60% 40% / 30% 60% 40% 70%; }
  75%       { border-radius: 30% 70% 40% 60% / 70% 40% 60% 30%; }
}
@keyframes photo-fly {
  0%   { opacity: 0; transform: scale(0.6) translateY(40px); }
  100% { opacity: 1; transform: scale(1) translateY(0); }
}

.decorative-dot {
  position: absolute;
  border-radius: 50%;
  animation: float 4s ease-in-out infinite;
}
.dot-1 {
  width: 60px; height: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  bottom: -20px; left: -30px;
  animation-delay: 0s;
}
.dot-2 {
  width: 70px; height: 70px;
  background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
  top: 20px; right: -40px;
  animation-delay: -1s;
}
.dot-3 {
  width: 50px; height: 50px;
  background: linear-gradient(135deg, #f9a8d4 0%, #a78bfa 100%);
  bottom: 40px; right: -25px;
  animation-delay: -2s;
}

/* ─── Scroll indicator ───────────────────────────────── */
.scroll-indicator {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  color: var(--subtext-color);
  font-size: 0.85rem;
  animation: bounce 2s ease-in-out infinite;
}
.scroll-arrow {
  width: 20px; height: 20px;
  border-right: 2px solid var(--subtext-color);
  border-bottom: 2px solid var(--subtext-color);
  transform: rotate(45deg);
}
@keyframes bounce {
  0%, 100% { transform: translateX(-50%) translateY(0); }
  50%       { transform: translateX(-50%) translateY(10px); }
}

/* ─── Responsive ─────────────────────────────────────── */
@media (max-width: 968px) {
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 3rem;
  }
  .hero-text { order: 2; }
  .hero-visual { order: 1; }
  .name { font-size: 3rem; }
  .name-3d-wrap { justify-content: center; }
  .title { font-size: 1.4rem; }
  .description { margin-left: auto; margin-right: auto; }
  .hero-buttons { justify-content: center; }
  .avatar-container { width: 250px; height: 250px; }
}
</style>
