<script setup>
import CountdownTimer from './components/CountdownTimer.vue'
import BigClock from './components/BigClock.vue'
import MemeCard from './components/MemeCard.vue'
import WeatherCard from './components/WeatherCard.vue'
import NewsCard from './components/NewsCard.vue'
</script>

<template>
  <div class="dashboard">
    <!-- Left: Countdown to Summer Break -->
    <aside class="col col-left">
      <CountdownTimer />
    </aside>

    <!-- Middle: Clock & Memes -->
    <main class="col col-mid">
      <BigClock />
      <MemeCard />
    </main>

    <!-- Right: Weather & News -->
    <aside class="col col-right">
      <WeatherCard />
      <NewsCard />
    </aside>
  </div>
</template>

<style scoped>
.dashboard {
  display: grid;
  grid-template-columns: 35% 35% 30%;
  height: 100vh;
  gap: 30px;
  padding: 28px;
  background: var(--bg-primary);
  box-sizing: border-box;
}

.col {
  display: flex;
  flex-direction: column;
  height: 100%;
  min-height: 0;
}

.col-mid {
  gap: 24px;
}

.col-right {
  gap: 24px;
}

@media (max-width: 1200px) {
  .dashboard {
    grid-template-columns: 1fr 1fr;
    overflow-y: auto;
  }
  .col-right { display: none; }
}
</style>
