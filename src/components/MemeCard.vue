<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const meme = ref(null)
const nextMeme = ref(null)
const loading = ref(true)
const error = ref(null)
const showCurrent = ref(true)
let rotationInterval = null

async function fetchMeme() {
  try {
    const response = await fetch('https://meme-api.com/gimme/ProgrammerHumor')
    if (!response.ok) throw new Error('Kunne ikke hente meme')
    const data = await response.json()
    return {
      title: data.title,
      url: data.url,
      postLink: data.postLink
    }
  } catch (err) {
    throw err
  }
}

async function loadInitialMeme() {
  try {
    meme.value = await fetchMeme()
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}

async function rotateMeme() {
  try {
    nextMeme.value = await fetchMeme()
    // Fade out current
    showCurrent.value = false
    // After fade-out, swap and fade-in
    setTimeout(() => {
      meme.value = nextMeme.value
      nextMeme.value = null
      showCurrent.value = true
    }, 600)
  } catch (err) {
    // Silently fail, keep current meme
  }
}

onMounted(() => {
  loadInitialMeme()
  rotationInterval = setInterval(rotateMeme, 300000) // 5 minutes
})

onUnmounted(() => {
  if (rotationInterval) clearInterval(rotationInterval)
})
</script>

<template>
  <div class="meme-card">
    <div class="meme-header">
      <span class="meme-label">PROGRAMMER HUMOR</span>
    </div>

    <div v-if="loading" class="meme-loading">
      <div class="spinner"></div>
      <p>Henter meme...</p>
    </div>

    <div v-else-if="error" class="meme-error">
      <p>{{ error }}</p>
    </div>

    <div v-else-if="meme" class="meme-content" :class="{ 'fade-out': !showCurrent }">
      <div class="meme-image-wrapper">
        <img :src="meme.url" :alt="meme.title" class="meme-image" />
      </div>
      <p class="meme-title">{{ meme.title }}</p>
    </div>
  </div>
</template>

<style scoped>
.meme-card {
  background: var(--bg-card);
  border-radius: var(--border-radius);
  box-shadow: var(--card-shadow);
  padding: 24px;
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.meme-header {
  margin-bottom: 16px;
}

.meme-label {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--accent-meme);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  background: rgba(233, 30, 99, 0.12);
  padding: 4px 12px;
  border-radius: 20px;
}

.meme-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  transition: opacity 0.6s ease;
}

.meme-content.fade-out {
  opacity: 0;
}

.meme-image-wrapper {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  max-height: 420px;
  overflow: hidden;
  border-radius: 12px;
  background: rgba(0, 0, 0, 0.2);
}

.meme-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

.meme-title {
  margin-top: 12px;
  font-size: 0.85rem;
  color: var(--text-secondary);
  text-align: center;
  line-height: 1.4;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.meme-loading, .meme-error {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12px;
  color: var(--text-muted);
}

.spinner {
  width: 28px;
  height: 28px;
  border: 2px solid var(--bg-tertiary);
  border-top-color: var(--accent-light);
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>
