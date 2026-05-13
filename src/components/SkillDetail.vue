<template>
  <div class="skill-modal" @click.self="close">
    <div class="skill-card">
      <button class="close-btn" @click="close" aria-label="Close">×</button>
      <div class="skill-image">
        <div class="image-inner">{{ displayImage }}</div>
      </div>
      <h3 class="skill-title">{{ skill.title }}</h3>
      <p class="skill-full">{{ skill.fullDescription || skill.description }}</p>
      <div v-if="skill.examples && skill.examples.length" class="examples">
        <h4>Example Work</h4>
        <ul>
          <li v-for="(ex, idx) in skill.examples" :key="idx">
            <a :href="ex.link" target="_blank" rel="noopener noreferrer">{{ ex.title }}</a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, computed } from 'vue'

const props = defineProps({
  skill: { type: Object, required: true }
})

const emit = defineEmits(['close'])

function close() {
  emit('close')
}

const displayImage = computed(() => {
  // if image is an emoji or short string, show it; otherwise leave as blank placeholder
  const img = props.skill.image || props.skill.icon || ''
  return img
})
</script>

<style scoped>
.skill-modal {
  position: fixed;
  inset: 0;
  display: grid;
  place-items: center;
  background: rgba(2,6,23,0.6);
  z-index: 99999;
}

.skill-card {
  width: min(760px, 92%);
  background: linear-gradient(180deg, #0f1220 0%, #171726 100%);
  border-radius: 16px;
  padding: 2rem;
  box-shadow: 0 30px 80px rgba(2,6,23,0.7);
  position: relative;
  color: #e6e6ee;
}

.close-btn {
  position: absolute;
  right: 12px;
  top: 12px;
  background: transparent;
  border: none;
  color: #cfcfe6;
  font-size: 1.6rem;
  cursor: pointer;
}

.skill-image {
  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}

.image-inner {
  width: 140px;
  height: 140px;
  border-radius: 12px;
  display: grid;
  place-items: center;
  font-size: 4rem;
  background: linear-gradient(135deg, rgba(102,126,234,0.12), rgba(122,84,255,0.12));
  color: #fff;
}

.skill-title {
  text-align: center;
  margin: 0 0 0.5rem 0;
  font-size: 1.6rem;
}

.skill-full {
  text-align: center;
  color: #a8a8b8;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.examples h4 { margin: 1rem 0 0.5rem 0; }
.examples ul { padding-left: 1.2rem; margin: 0; }
.examples a { color: #8ea0ff; text-decoration: none; }
</style>
