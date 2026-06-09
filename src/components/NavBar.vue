<script setup>
import { ref, onMounted, onUnmounted, inject } from 'vue'

const isScrolled = ref(false)
const isMenuOpen = ref(false)
const isDark = ref(true)

const locale = inject('locale')
const setLocale = inject('setLocale')
const t = inject('t')

const isLangDropdownOpen = ref(false)

const languages = [
  { code: 'en', name: 'English', flag: '🇬🇧' },
  { code: 'km', name: 'ភាសាខ្មែរ', flag: '🇰🇭' },
  { code: 'fr', name: 'Français', flag: '🇫🇷' }
]

const toggleLangDropdown = () => {
  isLangDropdownOpen.value = !isLangDropdownOpen.value
}

const selectLanguage = (code) => {
  setLocale(code)
  isLangDropdownOpen.value = false
}

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
  isMenuOpen.value = false
}

const toggleTheme = () => {
  isDark.value = !isDark.value
  const isLight = !isDark.value
  
  if (isLight) {
    document.documentElement.classList.add('light')
    localStorage.setItem('theme', 'light')
    window.dispatchEvent(new CustomEvent('theme-change', { detail: 'light' }))
  } else {
    document.documentElement.classList.remove('light')
    localStorage.setItem('theme', 'dark')
    window.dispatchEvent(new CustomEvent('theme-change', { detail: 'dark' }))
  }
}

const handleOutsideClick = (e) => {
  if (!e.target.closest('.lang-selector')) {
    isLangDropdownOpen.value = false
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  window.addEventListener('click', handleOutsideClick)
  
  // Set theme from localStorage or default system color scheme
  const savedTheme = localStorage.getItem('theme')
  const systemPrefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
  
  if (savedTheme === 'light' || (!savedTheme && !systemPrefersDark)) {
    isDark.value = false
    document.documentElement.classList.add('light')
    setTimeout(() => {
      window.dispatchEvent(new CustomEvent('theme-change', { detail: 'light' }))
    }, 100)
  } else {
    isDark.value = true
    document.documentElement.classList.remove('light')
    setTimeout(() => {
      window.dispatchEvent(new CustomEvent('theme-change', { detail: 'dark' }))
    }, 100)
  }
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  window.removeEventListener('click', handleOutsideClick)
})
</script>

<template>
  <nav class="navbar" :class="{ scrolled: isScrolled }">
    <div class="nav-container">
      <a href="#" class="logo" @click.prevent="scrollToSection('hero')">
        <span class="logo-text">Sana</span>
      </a>
      
      <div class="nav-right">
        <ul class="nav-links" :class="{ active: isMenuOpen }">
          <li><a href="#hero" @click.prevent="scrollToSection('hero')">{{ t('nav_home') }}</a></li>
          <li><a href="#about" @click.prevent="scrollToSection('about')">{{ t('nav_about') }}</a></li>
          <li><a href="#skills" @click.prevent="scrollToSection('skills')">{{ t('nav_skills') }}</a></li>
          <li><a href="#projects" @click.prevent="scrollToSection('projects')">{{ t('nav_projects') }}</a></li>
          <li><a href="#contact" @click.prevent="scrollToSection('contact')">{{ t('nav_contact') }}</a></li>
        </ul>

        <!-- Language Selector Dropdown -->
        <div class="lang-selector">
          <button class="lang-btn" @click.stop="toggleLangDropdown" aria-label="Select Language">
            <span class="lang-globe">🌐</span>
            <span class="lang-code">{{ locale.toUpperCase() }}</span>
            <span class="lang-chevron" :class="{ open: isLangDropdownOpen }">▼</span>
          </button>
          
          <Transition name="dropdown">
            <div v-if="isLangDropdownOpen" class="lang-dropdown">
              <button 
                v-for="lang in languages" 
                :key="lang.code"
                class="lang-option"
                :class="{ active: locale === lang.code }"
                @click="selectLanguage(lang.code)"
              >
                <span class="lang-flag">{{ lang.flag }}</span>
                <span class="lang-name">{{ lang.name }}</span>
                <span v-if="locale === lang.code" class="lang-check">✓</span>
              </button>
            </div>
          </Transition>
        </div>
        
        <button class="theme-toggle" @click="toggleTheme" aria-label="Toggle Theme">
          <svg v-if="isDark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="theme-icon sun">
            <path d="M12 2.25a.75.75 0 0 1 .75.75v2.25a.75.75 0 0 1-1.5 0V3a.75.75 0 0 1 .75-.75ZM6.166 5.106a.75.75 0 0 1 1.06 0l1.59 1.59a.75.75 0 1 1-1.06 1.06l-1.59-1.59a.75.75 0 0 1 0-1.06Zm11.668 0a.75.75 0 0 1 0 1.06l-1.59 1.59a.75.75 0 1 1-1.06-1.06l1.59-1.59a.75.75 0 0 1 1.06 0ZM12 6.75a5.25 5.25 0 1 0 0 10.5 5.25 5.25 0 0 0 0-10.5Zm-8.25 5.25a.75.75 0 0 1 .75-.75h2.25a.75.75 0 0 1 0 1.5H4.5a.75.75 0 0 1-.75-.75Zm12.75 0a.75.75 0 0 1 .75-.75h2.25a.75.75 0 0 1 0 1.5h-2.25a.75.75 0 0 1-.75-.75ZM6.166 18.894a.75.75 0 0 1 0-1.06l1.59-1.59a.75.75 0 1 1 1.06 1.06l-1.59 1.59a.75.75 0 0 1-1.06 0Zm11.668 0a.75.75 0 0 1-1.06-1.06l1.59-1.59a.75.75 0 1 1 1.06 1.06l-1.59 1.59a.75.75 0 0 1 0 1.06ZM12 18.75a.75.75 0 0 1 .75.75V21a.75.75 0 0 1-1.5 0v-1.5a.75.75 0 0 1 .75-.75Z"/>
          </svg>
          <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="theme-icon moon">
            <path fill-rule="evenodd" d="M9.528 1.718a.75.75 0 0 1 .162.819A8.97 8.97 0 0 0 9 6a9 9 0 0 0 9 9 8.97 8.97 0 0 0 3.463-.69.75.75 0 0 1 .981.98 10.503 10.503 0 1 1-16.9-11.54a.75.75 0 0 1 .818-.162Z" clip-rule="evenodd"/>
          </svg>
        </button>
        
        <button class="menu-toggle" @click="isMenuOpen = !isMenuOpen" :class="{ active: isMenuOpen }">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  padding: 1rem 2rem;
  transition: all 0.3s ease;
  background: transparent;
}

.navbar.scrolled {
  background: var(--nav-bg);
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.15);
}

.nav-container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  text-decoration: none;
}

.logo-text {
  font-size: 1.8rem;
  font-weight: 700;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.nav-right {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 2rem;
  margin: 0;
  padding: 0;
}

.nav-links a {
  color: var(--nav-link);
  text-decoration: none;
  font-weight: 500;
  position: relative;
  padding: 0.5rem 0;
}

.nav-links a::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  transition: width 0.3s ease;
}

.nav-links a:hover {
  color: var(--nav-link-hover);
}

.nav-links a:hover::after {
  width: 100%;
}

.theme-toggle {
  background: none;
  border: none;
  cursor: pointer;
  width: 38px;
  height: 38px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-color);
  background: var(--card-bg);
  border: 1px solid var(--card-border);
}

.theme-toggle:hover {
  transform: scale(1.08);
  color: #667eea;
  border-color: rgba(102, 126, 234, 0.4);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.15);
}

.theme-icon {
  width: 18px;
  height: 18px;
  transition: transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.theme-toggle:hover .theme-icon {
  transform: rotate(20deg);
}

.menu-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
}

.menu-toggle span {
  width: 25px;
  height: 3px;
  background: var(--text-color);
  border-radius: 2px;
  transition: all 0.3s ease;
}

.menu-toggle.active span:nth-child(1) {
  transform: rotate(45deg) translate(5px, 5px);
}

.menu-toggle.active span:nth-child(2) {
  opacity: 0;
}

.menu-toggle.active span:nth-child(3) {
  transform: rotate(-45deg) translate(7px, -6px);
}

/* Language Selector styles */
.lang-selector {
  position: relative;
}

.lang-btn {
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  color: var(--text-color);
  padding: 0.5rem 1rem;
  border-radius: 50px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
  font-weight: 600;
  transition: all 0.3s ease;
  height: 38px;
}

.lang-btn:hover {
  transform: scale(1.04);
  border-color: rgba(102, 126, 234, 0.4);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.15);
}

.lang-globe {
  font-size: 1rem;
}

.lang-code {
  font-size: 0.85rem;
  letter-spacing: 1px;
}

.lang-chevron {
  font-size: 0.6rem;
  transition: transform 0.3s ease;
  opacity: 0.7;
}

.lang-chevron.open {
  transform: rotate(180deg);
}

.lang-dropdown {
  position: absolute;
  top: 100%;
  right: 0;
  margin-top: 0.5rem;
  background: var(--nav-bg);
  backdrop-filter: blur(15px);
  border: 1px solid var(--card-border);
  border-radius: 12px;
  padding: 0.5rem;
  width: 150px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  z-index: 1001;
}

.lang-option {
  background: transparent;
  border: none;
  color: var(--text-color);
  padding: 0.6rem 0.8rem;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  font-size: 0.9rem;
  text-align: left;
  transition: all 0.2s ease;
  width: 100%;
}

.lang-option:hover {
  background: rgba(102, 126, 234, 0.1);
  color: #667eea;
}

.lang-option.active {
  background: rgba(102, 126, 234, 0.15);
  color: #667eea;
  font-weight: 600;
}

.lang-flag {
  font-size: 1.1rem;
}

.lang-name {
  flex: 1;
}

.lang-check {
  font-size: 0.8rem;
  font-weight: 700;
}

/* Vue Dropdown Transitions */
.dropdown-enter-active,
.dropdown-leave-active {
  transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
}

.dropdown-enter-from,
.dropdown-leave-to {
  opacity: 0;
  transform: translateY(-8px) scale(0.95);
}

@media (max-width: 768px) {
  .menu-toggle {
    display: flex;
  }

  .nav-links {
    position: fixed;
    top: 70px;
    left: 0;
    right: 0;
    background: var(--nav-bg);
    border-bottom: 1px solid var(--card-border);
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    gap: 1.5rem;
    transform: translateY(-150%);
    transition: transform 0.3s ease;
  }

  .nav-links.active {
    transform: translateY(0);
  }
}
</style>
