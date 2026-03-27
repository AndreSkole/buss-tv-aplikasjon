<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const currentTime = ref(new Date())
let clockInterval = null

onMounted(() => {
  clockInterval = setInterval(() => {
    currentTime.value = new Date()
  }, 1000)
})

onUnmounted(() => {
  if (clockInterval) clearInterval(clockInterval)
})

const pad = (n) => n.toString().padStart(2, '0')
const hours = () => pad(currentTime.value.getHours())
const minutes = () => pad(currentTime.value.getMinutes())
const seconds = () => pad(currentTime.value.getSeconds())

const formattedDate = () => {
  return currentTime.value.toLocaleDateString('nb-NO', {
    weekday: 'long',
    day: 'numeric',
    month: 'long',
    year: 'numeric'
  })
}
</script>

<template>
  <div class="big-clock">
    <div class="time-display">
      <span class="digit">{{ hours() }}</span>
      <span class="sep">:</span>
      <span class="digit">{{ minutes() }}</span>
      <span class="sep">:</span>
      <span class="digit sec">{{ seconds() }}</span>
    </div>
    <div class="date-display">{{ formattedDate() }}</div>
  </div>
</template>

<style scoped>
.big-clock {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 48px 20px 36px;
  background: var(--bg-card);
  border-radius: var(--border-radius);
  box-shadow: var(--card-shadow);
}

.time-display {
  display: flex;
  align-items: baseline;
  font-family: 'Inter', sans-serif;
  font-weight: 200;
  font-size: 9rem;
  line-height: 1;
  color: var(--text-primary);
  letter-spacing: -0.04em;
}

.digit {
  min-width: 1.1ch;
  text-align: center;
}

.sep {
  color: var(--accent-light);
  margin: 0 0.05ch;
  opacity: 0.6;
  animation: blink 2s ease-in-out infinite;
}

.sec {
  font-size: 5.5rem;
  color: var(--text-muted);
  font-weight: 100;
}

.date-display {
  margin-top: 12px;
  font-size: 1.3rem;
  font-weight: 400;
  color: var(--text-secondary);
  text-transform: capitalize;
}

@keyframes blink {
  0%, 100% { opacity: 0.6; }
  50% { opacity: 0.15; }
}

@media (max-width: 1400px) {
  .time-display { font-size: 6rem; }
  .sec { font-size: 4rem; }
}
</style>
