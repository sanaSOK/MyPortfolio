<template>
  <button
    v-show="visible"
    @click="scrollToTop"
    class="scroll-top"
    aria-label="Scroll to top"
  >
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M12 5l-7 7h4v7h6v-7h4l-7-7z" fill="white"/>
    </svg>
  </button>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const visible = ref(false)

function onScroll() {
  visible.value = window.scrollY > 250
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

onMounted(() => {
  window.addEventListener('scroll', onScroll, { passive: true })
  onScroll()
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll)
})
</script>

<style scoped>
.scroll-top {
  position: fixed;
  right: 28px;
  bottom: 28px;
  width: 56px;
  height: 56px;
  border-radius: 999px;
  display: grid;
  place-items: center;
  background: linear-gradient(135deg, #6b7cff 0%, #5ad0ff 100%);
  box-shadow: 0 8px 24px rgba(91, 105, 255, 0.18);
  border: none;
  cursor: pointer;
  transform: translateY(0);
  transition: transform 0.18s ease, opacity 0.18s ease;
  z-index: 9999;
}

.scroll-top:hover {
  transform: translateY(-4px);
}

.scroll-top[style*="display: none"] {
  opacity: 0;
  pointer-events: none;
}

/* small screens adjust spacing */
@media (max-width: 640px) {
  .scroll-top { right: 18px; bottom: 18px; width: 48px; height: 48px; }
}
</style>
