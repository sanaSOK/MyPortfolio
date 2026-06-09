<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

const canvasRef = ref(null)

let animationFrameId = null
let width = 0
let height = 0
let time = 0
let frameCount = 0

let particles = []
const lastMouse = { x: null, y: null }
let isInteracting = false
let lastScrollY = window.scrollY

const isLightMode = ref(false)

const handleThemeChange = (e) => {
  isLightMode.value = e.detail === 'light'
}

class SmokePuff {
  constructor(x, y, isGentleWisp = false) {
    this.x = x
    this.y = y
    this.life = 0
    this.maxLife = isGentleWisp ? Math.random() * 90 + 110 : Math.random() * 60 + 80
    
    // Slow drifting velocities
    this.vx = (Math.random() - 0.5) * (isGentleWisp ? 0.3 : 1.0)
    // Smoke drifts upwards
    this.vy = -Math.random() * (isGentleWisp ? 0.6 : 1.4) - 0.4
    
    // Particle size starts small and expands like puffing smoke clouds
    this.startSize = isGentleWisp ? Math.random() * 4 + 3 : Math.random() * 10 + 8
    this.size = this.startSize
    this.maxSize = isGentleWisp ? Math.random() * 22 + 25 : Math.random() * 55 + 65
    
    // Rotational factors to spin the cloud cluster over time
    this.rotation = Math.random() * Math.PI * 2
    this.rotationSpeed = (Math.random() - 0.5) * 0.015
    
    // Sinusoidal curling wobble values
    this.wobbleSpeed = Math.random() * 0.06 + 0.02
    this.wobblePhase = Math.random() * Math.PI * 2
    
    // Opacity
    this.alphaBase = isGentleWisp ? 0.35 : 0.55
    this.alpha = this.alphaBase
  }

  update() {
    this.life++
    
    // Drift positions
    this.x += this.vx
    this.y += this.vy
    
    // Apply curl wobble
    this.x += Math.sin(this.life * this.wobbleSpeed + this.wobblePhase) * 0.35
    
    // Spin cloud cluster
    this.rotation += this.rotationSpeed
    
    // Expand smoke cloud wisp
    const ratio = this.life / this.maxLife
    this.size = this.startSize + (this.maxSize - this.startSize) * ratio
    
    // Fade out wisp
    this.alpha = (1 - ratio) * this.alphaBase
    
    // Air friction deceleration
    this.vx *= 0.985
    this.vy *= 0.985
  }

  draw(ctx) {
    if (this.alpha <= 0) return
    
    ctx.save()
    // Move coordinate origin to particle center and rotate for spinning cloud
    ctx.translate(this.x, this.y)
    ctx.rotate(this.rotation)
    
    // Draw 3 offset sub-puffs to create an organic, fluffy cloud shape instead of a perfect circle
    for (let j = 0; j < 3; j++) {
      const angle = (j * Math.PI * 2) / 3 + this.rotation * 0.2
      // Spreading radius scales as the particle expands
      const offsetDist = this.size * 0.22
      const ox = Math.cos(angle) * offsetDist
      const oy = Math.sin(angle) * offsetDist
      
      const r = this.size * (0.65 + (j % 2) * 0.15)
      
      const gradient = ctx.createRadialGradient(ox, oy, 0, ox, oy, r)
      
      // Select smoke colors based on active theme
      const centerColor = isLightMode.value ? '40, 42, 45' : '238, 240, 245'
      const midColor = isLightMode.value ? '80, 85, 95' : '182, 187, 197'
      const edgeColor = isLightMode.value ? '120, 125, 135' : '132, 137, 147'
      
      // Soft grey-silver smoke in dark mode, charcoal-soot smoke in light mode
      gradient.addColorStop(0, `rgba(${centerColor}, ${this.alpha * 0.7})`)
      gradient.addColorStop(0.35, `rgba(${midColor}, ${this.alpha * 0.42})`)
      gradient.addColorStop(0.7, `rgba(${edgeColor}, ${this.alpha * 0.14})`)
      gradient.addColorStop(1, `rgba(${edgeColor}, 0)`)
      
      ctx.fillStyle = gradient
      ctx.beginPath()
      ctx.arc(ox, oy, r, 0, Math.PI * 2)
      ctx.fill()
    }
    
    ctx.restore()
  }
}

const handleResize = () => {
  if (!canvasRef.value) return
  const canvas = canvasRef.value
  width = canvas.width = window.innerWidth
  height = canvas.height = window.innerHeight
}

const handleMouseDown = (e) => {
  isInteracting = true
  lastMouse.x = e.clientX
  lastMouse.y = e.clientY
  particles.push(new SmokePuff(lastMouse.x, lastMouse.y))
}

const handleMouseUp = () => {
  isInteracting = false
  lastMouse.x = null
  lastMouse.y = null
}

const handleTouchStart = (e) => {
  isInteracting = true
  if (e.touches && e.touches.length > 0) {
    const touch = e.touches[0]
    lastMouse.x = touch.clientX
    lastMouse.y = touch.clientY
    particles.push(new SmokePuff(lastMouse.x, lastMouse.y))
  }
}

const handleMouseMove = (e) => {
  if (!isInteracting) return
  const x = e.clientX
  const y = e.clientY
  
  if (lastMouse.x === null) {
    lastMouse.x = x
    lastMouse.y = y
    return
  }
  
  spawnSmoke(x, y)
}

const handleTouchMove = (e) => {
  if (!isInteracting) return
  if (e.touches && e.touches.length > 0) {
    const touch = e.touches[0]
    const x = touch.clientX
    const y = touch.clientY
    
    if (lastMouse.x === null) {
      lastMouse.x = x
      lastMouse.y = y
      return
    }
    
    spawnSmoke(x, y)
  }
}

const spawnSmoke = (x, y) => {
  const distance = Math.hypot(x - lastMouse.x, y - lastMouse.y)
  
  if (distance > 15) {
    const steps = Math.min(Math.floor(distance / 20), 4) || 1
    for (let i = 0; i < steps; i++) {
      const t = i / steps
      const px = lastMouse.x + (x - lastMouse.x) * t
      const py = lastMouse.y + (y - lastMouse.y) * t
      
      const rx = px + (Math.random() - 0.5) * 12
      const ry = py + (Math.random() - 0.5) * 12
      
      particles.push(new SmokePuff(rx, ry))
    }
    
    lastMouse.x = x
    lastMouse.y = y
  }
}

const handleScroll = () => {
  const currentScrollY = window.scrollY
  const scrollDelta = currentScrollY - lastScrollY
  lastScrollY = currentScrollY

  // Draft force from scroll pushes puffs vertically
  particles.forEach(p => {
    p.y -= scrollDelta * 0.85
    p.vx += (Math.random() - 0.5) * (Math.abs(scrollDelta) * 0.035)
  })
}

const animate = () => {
  if (!canvasRef.value) return
  const canvas = canvasRef.value
  const ctx = canvas.getContext('2d')
  
  ctx.clearRect(0, 0, width, height)
  
  time++
  frameCount++
  
  // Update and draw all active puffs
  particles = particles.filter(p => {
    p.update()
    p.draw(ctx)
    return p.life < p.maxLife
  })
  
  animationFrameId = requestAnimationFrame(animate)
}

onMounted(() => {
  isLightMode.value = document.documentElement.classList.contains('light')
  handleResize()
  window.addEventListener('resize', handleResize)
  window.addEventListener('mousedown', handleMouseDown)
  window.addEventListener('mouseup', handleMouseUp)
  window.addEventListener('mouseleave', handleMouseUp)
  window.addEventListener('mousemove', handleMouseMove)
  window.addEventListener('touchstart', handleTouchStart, { passive: true })
  window.addEventListener('touchmove', handleTouchMove, { passive: true })
  window.addEventListener('touchend', handleMouseUp)
  window.addEventListener('scroll', handleScroll, { passive: true })
  window.addEventListener('theme-change', handleThemeChange)
  animate()
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  window.removeEventListener('mousedown', handleMouseDown)
  window.removeEventListener('mouseup', handleMouseUp)
  window.removeEventListener('mouseleave', handleMouseUp)
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('touchstart', handleTouchStart)
  window.removeEventListener('touchmove', handleTouchMove)
  window.removeEventListener('touchend', handleMouseUp)
  window.removeEventListener('scroll', handleScroll)
  window.removeEventListener('theme-change', handleThemeChange)
  cancelAnimationFrame(animationFrameId)
})
</script>

<template>
  <canvas ref="canvasRef" class="smoke-canvas"></canvas>
</template>

<style scoped>
.smoke-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: -1;
  pointer-events: none;
}
</style>
