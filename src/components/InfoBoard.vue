<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const items = [
  { type: 'joke', content: 'Hvorfor liker ikke programmerere utendørsaktiviteter? Fordi det er for mange bugs der.' },
  { type: 'joke', content: 'En SQL-forespørsel går inn i en bar, går bort til to bord og spør: "Kan jeg få sitte her med dere?"' },
  { type: 'joke', content: 'Hvordan ser man om en person er en ekte ekstrovert systemutvikler? Han ser på DINE sko når han snakker med deg.' },
  { type: 'joke', content: 'Programmerer (n.): En organisme som konverterer kaffe til kode.' },
  { type: 'joke', content: 'Det finnes 10 typer mennesker i verden: De som forstår binært, og de som ikke gjør det.' },
  { type: 'quote', content: '"Talk is cheap. Show me the code." – Linus Torvalds' },
  { type: 'quote', content: '"Programs must be written for people to read, and only incidentally for machines to execute." – Abelson & Sussman' },
  { type: 'tip', content: 'Linux Tips: Bruk "ctrl + r" i terminalen for å søke i kommandohistorikken din.' },
  { type: 'tip', content: 'Linux Tips: "man [kommando]" er din beste venn når du lurer på hvordan noe fungerer.' },
  { type: 'tip', content: 'Linux Tips: "sudo !!" kjører den forrige kommandoen din med root-tilgang.' },
  { type: 'tip', content: 'Linux Tips: Bruk "alias" for å lage snarveier til lange kommandoer i .bashrc.' },
  { type: 'joke', content: 'Hvorfor krysset kyllingen veien? For å komme til den andre siden av Bit-en.' },
  { type: 'quote', content: '"Premature optimization is the root of all evil." – Donald Knuth' },
  { type: 'tip', content: 'Linux Tips: "htop" gir en mye bedre oversikt over prosesser enn vanlige "top".' }
]

const currentIndex = ref(0)
const isVisible = ref(true)
let rotationInterval = null

function rotate() {
  isVisible.value = false
  setTimeout(() => {
    currentIndex.value = (currentIndex.value + 1) % items.length
    isVisible.value = true
  }, 500)
}

onMounted(() => {
  rotationInterval = setInterval(rotate, 15000)
})

onUnmounted(() => {
  if (rotationInterval) clearInterval(rotationInterval)
})
</script>

<template>
  <div class="info-board">
    <div class="board-header">
      <span class="badge" :class="items[currentIndex].type">
        {{ items[currentIndex].type === 'joke' ? 'IT-VITS' : items[currentIndex].type === 'quote' ? 'SITAT' : 'LINUX-TIPS' }}
      </span>
    </div>
    <div class="board-content" :class="{ 'fade-out': !isVisible }">
      <p class="content-text">{{ items[currentIndex].content }}</p>
    </div>
  </div>
</template>

<style scoped>
.info-board {
  background: var(--bg-secondary);
  border-radius: var(--border-radius);
  padding: 32px;
  min-height: 240px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  box-shadow: var(--card-shadow);
  border-left: 6px solid var(--accent-contrast);
}

.board-header {
  position: absolute;
  top: 16px;
  left: 24px;
}

.badge {
  font-size: 0.75rem;
  font-weight: 800;
  padding: 4px 12px;
  border-radius: 20px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.badge.joke { background: var(--accent-contrast); color: white; }
.badge.quote { background: var(--accent); color: white; }
.badge.tip { background: var(--success); color: white; }

.board-content {
  transition: opacity 0.5s ease;
}

.board-content.fade-out {
  opacity: 0;
}

.content-text {
  font-size: 1.8rem;
  font-weight: 500;
  line-height: 1.4;
  color: var(--text-primary);
  text-align: center;
}
</style>
