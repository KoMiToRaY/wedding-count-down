<script setup>
import { ref } from 'vue'

const hasStartGame = ref(false)
const score = ref(0)
const message = ref('')
const leaves = ref([])
const timePerLeaf = 13000

let timeoutIds = []

const spawnLeaf = () => {
  const id = `${Date.now()}-${Math.random()}`
  const left = `${Math.random() * 80 + 10}%`
  const delay = 0 // on ne met plus de delay dans le style directement

  const timeout = setTimeout(() => {
    if (leaves.value.find(leaf => leaf.id === id)) {
      resetScore()
    }
  }, timePerLeaf)

  leaves.value.push({ id, left, delay, timeout })
}

const spawnWave = () => {
  // entre 3 et 6 feuilles, chacune avec un petit décalage
  const count = Math.floor(Math.random() * 4) + 3

  for (let i = 0; i < count; i++) {
    const delay = Math.random() * 2000 + 500 // entre 0.5s et 2.5s
    const t = setTimeout(() => {
      spawnLeaf()
    }, delay + i * 500)
    timeoutIds.push(t)
  }
}

const catchLeaf = (id) => {
  const leaf = leaves.value.find((l) => l.id === id)
  if (!leaf) return
  hasStartGame.value = true
  score.value++
  message.value = '🍁 Bien joué !'

  clearTimeout(leaf.timeout)
  leaves.value = leaves.value.filter((l) => l.id !== id)

  if (leaves.value.length === 0) {
    setTimeout(spawnWave, 1000)
  }
}

const resetScore = () => {
  message.value = '😢 Raté ! Une feuille s’est échappée...'
  score.value = 0

  // Nettoyer tout
  leaves.value.forEach((leaf) => clearTimeout(leaf.timeout))
  leaves.value = []
  timeoutIds.forEach(clearTimeout)
  timeoutIds = []

  setTimeout(spawnWave, 1500)
}

// Lancer au démarrage
spawnWave()
</script>

<template>
  <!-- Feuilles -->
  <div class="fixed top-0 left-0 w-full h-full pointer-events-none overflow-hidden z-50">
    <div
      v-for="leaf in leaves"
      :key="leaf.id"
      class="absolute top-[-100px] text-3xl animate-leaf"
      :style="{
        left: leaf.left,
      }"
      @click="catchLeaf(leaf.id)"
      style="cursor: pointer; pointer-events: auto;"
    >
      🍁
    </div>
  </div>

  <!-- Score -->
  <div
    v-if="hasStartGame"
    class="fixed top-4 left-1/2 transform -translate-x-1/2 text-lg font-semibold bg-white/80 px-4 py-1 rounded-full text-orange-800 shadow z-50"
  >
    Score : {{ score }}
  </div>

  <!-- Message -->
  <div
    v-if="hasStartGame && message"
    class="fixed top-16 left-1/2 transform -translate-x-1/2 text-md bg-white/90 text-orange-700 px-4 py-2 rounded shadow z-50"
  >
    {{ message }}
  </div>
</template>

<style scoped>
@keyframes fall {
  0% {
    transform: translateY(-100px) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(120vh) rotate(360deg);
    opacity: 1;
  }
}

.animate-leaf {
  animation: fall 13s linear forwards;
}
</style>
