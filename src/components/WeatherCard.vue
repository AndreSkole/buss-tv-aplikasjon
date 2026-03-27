<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const weather = ref(null)
const loading = ref(true)
const error = ref(null)
let updateInterval = null

// Ålesund coordinates
const lat = 62.4722
const lon = 6.1495

async function fetchWeather() {
  try {
    const response = await fetch(`https://api.met.no/weatherapi/locationforecast/2.0/compact?lat=${lat}&lon=${lon}`, {
      headers: {
        'User-Agent': 'ITLabDashboard/1.0 (andhen12@skole.mrfylke.no)'
      }
    })
    
    if (!response.ok) throw new Error('Kunne ikke hente værdata')
    
    const data = await response.json()
    const current = data.properties.timeseries[0].data.instant.details
    const next1h = data.properties.timeseries[0].data.next_1_hours
    
    weather.value = {
      temp: Math.round(current.air_temperature),
      wind: current.wind_speed,
      symbol: next1h?.summary.symbol_code || 'clearsky_day'
    }
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchWeather()
  updateInterval = setInterval(fetchWeather, 900000) // 15 minutes
})

onUnmounted(() => {
  if (updateInterval) clearInterval(updateInterval)
})

const getSymbolUrl = (code) => `https://raw.githubusercontent.com/metno/weathericons/main/weather/svg/${code}.svg`

</script>

<template>
  <div class="weather-card">
    <div class="card-header">
      <h2 class="card-title">Været i Ålesund</h2>
    </div>
    
    <div class="card-body">
      <div v-if="loading" class="loading">Laster vær...</div>
      <div v-else-if="error" class="error">{{ error }}</div>
      <div v-else class="weather-info">
        <div class="temp-section">
          <img :src="getSymbolUrl(weather.symbol)" class="weather-icon" alt="Værsymbol">
          <span class="temperature">{{ weather.temp }}°</span>
        </div>
        <div class="details">
          <div class="detail-item">
            <span class="label">Vind</span>
            <span class="value">{{ weather.wind }} m/s</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.weather-card {
  background: var(--bg-card);
  border-radius: var(--border-radius);
  padding: 24px;
  box-shadow: var(--card-shadow);
  margin-bottom: 24px;
}

.card-title {
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--text-secondary);
  margin-bottom: 16px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.weather-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.temp-section {
  display: flex;
  align-items: center;
  gap: 16px;
}

.weather-icon {
  width: 80px;
  height: 80px;
  filter: drop-shadow(0 2px 4px rgba(0,0,0,0.2));
}

.temperature {
  font-size: 4rem;
  font-weight: 800;
  color: var(--text-primary);
}

.details {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.detail-item {
  display: flex;
  flex-direction: column;
}

.label {
  font-size: 0.8rem;
  color: var(--text-muted);
  text-transform: uppercase;
}

.value {
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--text-primary);
}

.loading, .error {
  padding: 20px;
  text-align: center;
  color: var(--text-secondary);
}
</style>
