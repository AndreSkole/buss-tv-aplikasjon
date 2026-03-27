<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const newsItems = ref([])
const loading = ref(true)
const error = ref(null)
let refreshInterval = null

// Simulated IT news feed (Norwegian tech news style)
// In a production app you'd proxy an RSS feed or use a news API.
const allNews = [
  { title: 'OpenAI lanserer ny AI-modell som revolusjonerer kodegenerering', source: 'Kode24', time: '32 min siden', url: '#' },
  { title: 'NTNU-studenter vinner internasjonal hackathon med klimaløsning', source: 'Digi.no', time: '1 time siden', url: '#' },
  { title: 'Kubernetes 2.0: Ny versjon lover enklere oppsett for utviklere', source: 'Kode24', time: '2 timer siden', url: '#' },
  { title: 'Regjeringen satser 500 millioner på digital kompetanse i skolen', source: 'Digi.no', time: '3 timer siden', url: '#' },
  { title: 'Rust slår Python som mest populære språk på Stack Overflow', source: 'Kode24', time: '4 timer siden', url: '#' },
  { title: 'Norsk selskap utvikler AI-drevet løsning for offentlig sektor', source: 'Digi.no', time: '5 timer siden', url: '#' },
  { title: 'GitHub Copilot får stor oppdatering med støtte for hele prosjekter', source: 'Kode24', time: '6 timer siden', url: '#' },
  { title: 'Europas største datasenter planlagt i Nord-Norge', source: 'Digi.no', time: '7 timer siden', url: '#' },
  { title: 'Firefox 200 lansert med innebygd AI-oversetter og VPN', source: 'Kode24', time: '8 timer siden', url: '#' },
  { title: 'Ny undersøkelse: 7 av 10 norske bedrifter bruker skyløsninger', source: 'Digi.no', time: '9 timer siden', url: '#' },
  { title: 'Linux-kjernen 7.0 sluppet med bedre maskinvarestøtte', source: 'Kode24', time: '10 timer siden', url: '#' },
  { title: 'Telenor åpner ny cybersikkerhetshub i Ålesund', source: 'Digi.no', time: '11 timer siden', url: '#' },
]

function pickNews() {
  // Shuffle and pick 4
  const shuffled = [...allNews].sort(() => Math.random() - 0.5)
  newsItems.value = shuffled.slice(0, 5)
}

onMounted(() => {
  pickNews()
  loading.value = false
  refreshInterval = setInterval(pickNews, 600000) // Refresh every 10 min
})

onUnmounted(() => {
  if (refreshInterval) clearInterval(refreshInterval)
})
</script>

<template>
  <div class="news-card">
    <div class="news-header">
      <h2 class="card-title">IT-Nyheter</h2>
    </div>

    <div v-if="loading" class="news-loading">
      <p>Laster nyheter...</p>
    </div>

    <ul v-else class="news-list">
      <li v-for="(item, index) in newsItems" :key="index" class="news-item">
        <div class="news-source-badge" :class="item.source === 'Kode24' ? 'kode24' : 'digi'">
          {{ item.source }}
        </div>
        <a :href="item.url" class="news-title" target="_blank" rel="noopener">
          {{ item.title }}
        </a>
        <span class="news-time">{{ item.time }}</span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.news-card {
  background: var(--bg-card);
  border-radius: var(--border-radius);
  box-shadow: var(--card-shadow);
  padding: 24px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.card-title {
  font-size: 1rem;
  font-weight: 700;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 16px;
}

.news-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.news-item {
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--bg-tertiary);
}

.news-item:last-child {
  border-bottom: none;
  padding-bottom: 0;
}

.news-source-badge {
  font-size: 0.65rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: 2px 8px;
  border-radius: 4px;
  width: fit-content;
}

.news-source-badge.kode24 {
  background: rgba(74, 144, 217, 0.15);
  color: var(--accent-light);
}

.news-source-badge.digi {
  background: rgba(76, 175, 80, 0.15);
  color: var(--success);
}

.news-title {
  font-size: 0.95rem;
  font-weight: 500;
  color: var(--text-primary);
  text-decoration: none;
  line-height: 1.4;
  transition: color 0.2s;
}

.news-title:hover {
  color: var(--accent-light);
}

.news-time {
  font-size: 0.72rem;
  color: var(--text-muted);
}

.news-loading {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-muted);
}
</style>
