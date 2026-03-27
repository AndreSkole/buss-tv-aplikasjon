<script setup>
import { ref, onMounted } from 'vue'

const facts = [
  'Verdens første programmerer var en kvinne ved navn Ada Lovelace.',
  'Det første dataprogrammet ble laget for en mekanisk datamaskin kalt "Analytical Engine".',
  'Uttrykket "bug" oppstod da en moth (nattsvermer) ble funnet inni en Harvard Mark II-maskin i 1947.',
  'Over 90 % av verdens valuta finnes i dag kun på datamaskiner.',
  'Det første domenenavnet som ble registrert var symbolics.com i 1985.',
  'Linux-kjernen er verdens største samarbeidsprosjekt innen programvare.',
  'QWERTY-tastaturet ble designet for å bremse ned skrivehastigheten for å unngå at mekaniske skrivemaskiner låste seg.',
  'Verdens første nettside er fortsatt tilgjengelig og ble publisert av CERN i 1991.',
  'Omkring 30 000 nettsider blir hacket hver eneste dag på verdensbasis.',
  'Den første datamusen ble laget av tre i 1964 av Douglas Engelbart.',
  'Python-språket er oppkalt etter komediegruppen Monty Python, ikke slangen.',
  'Siri ble opprinnelig utviklet som et prosjekt av SRI International for det amerikanske forsvaret.',
  'En moderne smarttelefon har mer regnekraft enn alle NASA-datamaskinene som ble brukt til månelandingen til sammen.'
]

const randomFact = ref('')

onMounted(() => {
  randomFact.value = facts[Math.floor(Math.random() * facts.length)]
})
</script>

<template>
  <div class="tech-facts">
    <div class="card-header">
      <h2 class="card-title">Dagens Teknofakta</h2>
    </div>
    <div class="card-body">
      <p class="fact-text">{{ randomFact }}</p>
    </div>
  </div>
</template>

<style scoped>
.tech-facts {
  background: var(--bg-secondary);
  border-radius: var(--border-radius);
  padding: 24px;
  box-shadow: var(--card-shadow);
  flex: 1;
}

.card-title {
  font-size: 1rem;
  font-weight: 700;
  color: var(--text-secondary);
  margin-bottom: 16px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.fact-text {
  font-size: 1.25rem;
  font-weight: 500;
  line-height: 1.5;
  color: var(--text-primary);
  border-left: 4px solid var(--accent-light);
  padding-left: 16px;
}
</style>
