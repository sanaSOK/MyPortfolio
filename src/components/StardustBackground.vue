<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

const canvasRef = ref(null)

let animationFrameId = null
let width = 0
let height = 0
let time = 0
let particles = []
let clouds = []
const mouse = { x: null, y: null, radius: 160 }

const isLightMode = ref(false)

const darkColors = [
  'rgba(255, 255, 255, ',
  'rgba(102, 126, 234, ',
  'rgba(240, 147, 251, ',
  'rgba(79, 172, 254, '
]
const lightColors = [
  'rgba(26, 26, 46, ',
  'rgba(102, 126, 234, ',
  'rgba(118, 75, 162, ',
  'rgba(245, 87, 108, '
]

const handleThemeChange = (e) => {
  isLightMode.value = e.detail === 'light'
  const activePalette = isLightMode.value ? lightColors : darkColors
  particles.forEach(p => {
    const colorIndex = Math.floor(Math.random() * activePalette.length)
    p.colorBase = activePalette[colorIndex]
  })
}

class LightBeam {
  constructor() {
    this.angle = 20 * Math.PI / 180 // 20-degree incline slant
    this.sinAngle = Math.sin(this.angle)
    this.cosAngle = Math.cos(this.angle)
    this.tanAngle = Math.tan(this.angle)
    this.reset(true)
  }

  reset(initial = false) {
    // Spawn with extra horizontal padding since they fall diagonally
    this.x = Math.random() * (width + 350) - 250
    this.length = Math.random() * 80 + 35 // Beam length
    this.width = Math.random() * 1.3 + 0.7 // Narrow width
    
    this.lengthX = this.length * this.sinAngle
    this.lengthY = this.length * this.cosAngle
    
    // Spawn above the screen, taking lengthY into account
    this.y = initial ? Math.random() * height : -this.lengthY
    
    // Parallax vertical speed
    this.speedY = (this.length / 115) * 4.5 + 1.2
    // Horizontal speed matches the slant angle to align streaks with their trajectory
    this.speedX = this.speedY * this.tanAngle
    
    // Twinkling
    this.alpha = Math.random() * 0.4 + 0.15
    this.pulseSpeed = Math.random() * 0.008 + 0.003
    this.pulseDir = Math.random() > 0.5 ? 1 : -1
    
    const activePalette = isLightMode.value ? lightColors : darkColors
    const colorIndex = Math.floor(Math.random() * activePalette.length)
    this.colorBase = activePalette[colorIndex]
  }

  update() {
    // Falling diagonal motion
    this.y += this.speedY
    this.x += this.speedX

    // Magnetic warping
    if (mouse.x !== null && mouse.y !== null) {
      const dx = this.x - mouse.x
      const dy = this.y - mouse.y
      const distance = Math.hypot(dx, dy)
      
      if (distance < mouse.radius) {
        const force = (mouse.radius - distance) / mouse.radius
        this.x += (dx > 0 ? 1 : -1) * force * 5.5
      }
    }

    // Twinkling
    this.alpha += this.pulseSpeed * this.pulseDir
    if (this.alpha > 0.75) {
      this.alpha = 0.75
      this.pulseDir = -1
    } else if (this.alpha < 0.1) {
      this.alpha = 0.1
      this.pulseDir = 1
    }

    // Reset if the tail drifts off the bottom or the right boundary
    if (this.y - this.lengthY > height + 10 || this.x - this.lengthX > width + 10) {
      this.reset(false)
    }
  }

  draw(ctx) {
    if (this.alpha <= 0) return
    
    ctx.save()
    
    // Draw slanted line with gradient fading upwards
    const gradient = ctx.createLinearGradient(
      this.x - this.lengthX, 
      this.y - this.lengthY, 
      this.x, 
      this.y
    )
    gradient.addColorStop(0, 'rgba(255, 255, 255, 0)')
    gradient.addColorStop(1, this.colorBase + this.alpha + ')')
    
    ctx.strokeStyle = gradient
    ctx.lineWidth = this.width
    ctx.beginPath()
    ctx.moveTo(this.x - this.lengthX, this.y - this.lengthY)
    ctx.lineTo(this.x, this.y)
    ctx.stroke()
    
    ctx.restore()
  }
}

// Cloud definition for Light Mode
class Cloud {
  constructor(x, y) {
    this.x = x
    this.y = y
    this.speedX = Math.random() * 0.18 + 0.1 // Slow horizontal drift
    this.scale = Math.random() * 0.45 + 0.5  // Fluffy sizing
    this.opacity = Math.random() * 0.15 + 0.22 // Subtle opacity
    
    // overlapping circles to draw cloud puff shape
    this.circles = [
      { ox: 0, oy: 0, r: 40 },
      { ox: -32, oy: 6, r: 28 },
      { ox: 32, oy: 6, r: 28 },
      { ox: -16, oy: -20, r: 32 },
      { ox: 16, oy: -20, r: 32 }
    ]
  }

  update() {
    this.x += this.speedX
    
    const cloudWidth = 140 * this.scale
    if (this.x - cloudWidth > width) {
      this.x = -cloudWidth
      this.y = Math.random() * (height * 0.35) + 50
    }
  }

  draw(ctx) {
    ctx.save()
    ctx.translate(this.x, this.y)
    
    // Draw fluffy white clouds with drop-shadow glow
    ctx.fillStyle = `rgba(255, 255, 255, ${this.opacity})`
    ctx.shadowBlur = 24
    ctx.shadowColor = 'rgba(255, 255, 255, 0.75)'
    
    ctx.beginPath()
    this.circles.forEach(c => {
      ctx.arc(c.ox * this.scale, c.oy * this.scale, c.r * this.scale, 0, Math.PI * 2)
    })
    ctx.fill()
    ctx.restore()
  }
}

const initClouds = () => {
  clouds = []
  if (width === 0) return
  // Generate 5 clouds distributed across screen width
  for (let i = 0; i < 5; i++) {
    const cx = (width / 5) * i + Math.random() * (width * 0.1)
    const cy = Math.random() * (height * 0.35) + 60
    clouds.push(new Cloud(cx, cy))
  }
}

const drawSun = (ctx) => {
  ctx.save()
  
  const sunX = width - 150
  const sunY = 150
  
  // Corona pulsing sunbeam rays
  const pulse = Math.sin(time * 0.015) * 6
  const sunRadius = 42
  const glowRadius = 130 + pulse
  
  const gradient = ctx.createRadialGradient(sunX, sunY, sunRadius - 10, sunX, sunY, glowRadius)
  gradient.addColorStop(0, 'rgba(255, 245, 190, 0.85)')
  gradient.addColorStop(0.25, 'rgba(255, 215, 80, 0.45)')
  gradient.addColorStop(0.55, 'rgba(255, 180, 50, 0.15)')
  gradient.addColorStop(1, 'rgba(255, 180, 50, 0)')
  
  ctx.fillStyle = gradient
  ctx.beginPath()
  ctx.arc(sunX, sunY, glowRadius, 0, Math.PI * 2)
  ctx.fill()
  
  ctx.restore()
}

const handleResize = () => {
  if (!canvasRef.value) return
  const canvas = canvasRef.value
  width = canvas.width = window.innerWidth
  height = canvas.height = window.innerHeight
  
  const count = width < 768 ? 35 : 110
  particles = []
  for (let i = 0; i < count; i++) {
    particles.push(new LightBeam())
  }
  
  initClouds()
}

const handleMouseMove = (e) => {
  mouse.x = e.clientX
  mouse.y = e.clientY
}

const handleMouseLeave = () => {
  mouse.x = null
  mouse.y = null
}

const animate = () => {
  if (!canvasRef.value) return
  const canvas = canvasRef.value
  const ctx = canvas.getContext('2d')
  
  ctx.clearRect(0, 0, width, height)
  
  time++
  
  if (isLightMode.value) {
    // Light Mode: draw sun and clouds, no light streaks
    drawSun(ctx)
    clouds.forEach(c => {
      c.update()
      c.draw(ctx)
    })
  } else {
    // Dark Mode: draw falling vertical light streaks (beams of light)
    particles.forEach(p => {
      p.update()
      p.draw(ctx)
    })
  }
  
  animationFrameId = requestAnimationFrame(animate)
}

onMounted(() => {
  isLightMode.value = document.documentElement.classList.contains('light')
  handleResize()
  window.addEventListener('resize', handleResize)
  window.addEventListener('mousemove', handleMouseMove)
  window.addEventListener('mouseleave', handleMouseLeave)
  window.addEventListener('theme-change', handleThemeChange)
  initClouds()
  animate()
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('mouseleave', handleMouseLeave)
  window.removeEventListener('theme-change', handleThemeChange)
  cancelAnimationFrame(animationFrameId)
})
</script>

<template>
  <canvas ref="canvasRef" class="stardust-canvas"></canvas>
</template>

<style scoped>
.stardust-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: -2;
  pointer-events: none;
}
</style>
