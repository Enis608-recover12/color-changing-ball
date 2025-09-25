<template>
  <div class="container">
    <canvas ref="canvas" width="400" height="400"></canvas>

    <div class="controls">
      <label>Speed: {{ speed }}</label>
      <input type="range" v-model="speed" min="1" max="20" />

      <label>Mode:</label>
      <select v-model="mode">
        <option value="rainbow">Rainbow</option>
        <option value="pulse">Pulse</option>
        <option value="chaos">Chaos</option>
      </select>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const canvas = ref(null)
const speed = ref(5)
const mode = ref('rainbow')

let hue = 0
let pulsePhase = 0

onMounted(() => {
  const ctx = canvas.value.getContext('2d')

  function drawBall() {
    ctx.clearRect(0, 0, 400, 400)
    ctx.beginPath()
    ctx.arc(200, 200, 50, 0, Math.PI * 2)

    let fillStyle = ''

    if (mode.value === 'rainbow') {
      fillStyle = `hsl(${hue}, 100%, 50%)`
      hue = (hue + speed.value) % 360
    } else if (mode.value === 'pulse') {
      const brightness = 50 + 30 * Math.sin(pulsePhase)
      fillStyle = `hsl(${hue}, 100%, ${brightness}%)`
      pulsePhase += 0.05 * speed.value
      hue = (hue + 1) % 360
    } else if (mode.value === 'chaos') {
      const r = Math.floor(Math.random() * 256)
      const g = Math.floor(Math.random() * 256)
      const b = Math.floor(Math.random() * 256)
      fillStyle = `rgb(${r}, ${g}, ${b})`
    }

    ctx.fillStyle = fillStyle
    ctx.fill()
    requestAnimationFrame(drawBall)
  }

  drawBall()
})
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #111;
  height: 100vh;
  color: white;
  font-family: sans-serif;
}

.controls {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 300px;
}
</style>
