<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Target: June 20, 2026 at 16:00 local time
const targetDate = new Date(2026, 5, 20, 16, 0, 0) // month is 0-indexed

const days = ref('00')
const hours = ref('00')
const minutes = ref('00')
const seconds = ref('00')
const isFinished = ref(false)

let tickInterval = null

function pad(n) {
  return n.toString().padStart(2, '0')
}

function updateCountdown() {
  const now = new Date()
  const diff = targetDate - now

  if (diff <= 0) {
    days.value = '00'
    hours.value = '00'
    minutes.value = '00'
    seconds.value = '00'
    isFinished.value = true
    if (tickInterval) clearInterval(tickInterval)
    return
  }

  const totalSeconds = Math.floor(diff / 1000)
  const d = Math.floor(totalSeconds / 86400)
  const h = Math.floor((totalSeconds % 86400) / 3600)
  const m = Math.floor((totalSeconds % 3600) / 60)
  const s = totalSeconds % 60

  days.value = pad(d)
  hours.value = pad(h)
  minutes.value = pad(m)
  seconds.value = pad(s)
}

onMounted(() => {
  updateCountdown()
  tickInterval = setInterval(updateCountdown, 1000)
})

onUnmounted(() => {
  if (tickInterval) clearInterval(tickInterval)
})
</script>

<template>
  <div class="countdown-card">
    <div class="countdown-label">COUNTDOWN TO SUMMER BREAK</div>

    <div v-if="isFinished" class="countdown-finished">
      <span class="finished-text">☀️ SOMMERFERIE! ☀️</span>
    </div>

    <div v-else class="countdown-display">
      <div class="countdown-segment">
        <span class="countdown-value">{{ days }}</span>
        <span class="countdown-unit">DAGER</span>
      </div>
      <span class="countdown-sep">:</span>
      <div class="countdown-segment">
        <span class="countdown-value">{{ hours }}</span>
        <span class="countdown-unit">TIMER</span>
      </div>
      <span class="countdown-sep">:</span>
      <div class="countdown-segment">
        <span class="countdown-value">{{ minutes }}</span>
        <span class="countdown-unit">MINUTTER</span>
      </div>
      <span class="countdown-sep">:</span>
      <div class="countdown-segment">
        <span class="countdown-value">{{ seconds }}</span>
        <span class="countdown-unit">SEKUNDER</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.countdown-card {
  background: var(--bg-card);
  border-radius: var(--border-radius);
  box-shadow: var(--card-shadow);
  padding: 48px 32px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex: 1;
  gap: 40px;
}

.countdown-label {
  font-family: 'Inter', sans-serif;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: #39FF14;
  opacity: 0.6;
}

.countdown-display {
  display: flex;
  align-items: center;
  gap: 8px;
}

.countdown-segment {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.countdown-value {
  font-family: 'Inter', sans-serif;
  font-weight: 200;
  font-size: 5.5rem;
  line-height: 1;
  color: #39FF14;
  letter-spacing: -0.02em;
  text-shadow: 0 0 20px rgba(57, 255, 20, 0.3), 0 0 60px rgba(57, 255, 20, 0.1);
}

.countdown-unit {
  font-family: 'Inter', sans-serif;
  font-size: 0.65rem;
  font-weight: 500;
  letter-spacing: 0.15em;
  color: #39FF14;
  opacity: 0.4;
  text-transform: uppercase;
}

.countdown-sep {
  font-family: 'Inter', sans-serif;
  font-weight: 100;
  font-size: 4rem;
  color: #39FF14;
  opacity: 0.3;
  margin-bottom: 24px;
}

.countdown-finished {
  display: flex;
  align-items: center;
  justify-content: center;
}

.finished-text {
  font-family: 'Inter', sans-serif;
  font-weight: 300;
  font-size: 3rem;
  color: #39FF14;
  text-shadow: 0 0 20px rgba(57, 255, 20, 0.4);
  animation: glow-pulse 2s ease-in-out infinite;
}

@keyframes glow-pulse {
  0%, 100% { text-shadow: 0 0 20px rgba(57, 255, 20, 0.4); }
  50% { text-shadow: 0 0 40px rgba(57, 255, 20, 0.7), 0 0 80px rgba(57, 255, 20, 0.3); }
}

@media (max-width: 1400px) {
  .countdown-value { font-size: 3.5rem; }
  .countdown-sep { font-size: 2.5rem; }
}
</style>
