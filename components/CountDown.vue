<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const targetDate = new Date('2026-09-24T11:00:00').getTime()

const days = ref(0)
const hours = ref(0)
const minutes = ref(0)
const seconds = ref(0)

const isReady = ref(false)
let interval = null

const updateCountdown = () => {
  const now = Date.now()
  const diff = targetDate - now

  if (diff <= 0) {
    clearInterval(interval)
    days.value = hours.value = minutes.value = seconds.value = 0
    isReady.value = true
    return
  }

  days.value = Math.floor(diff / (1000 * 60 * 60 * 24))
  hours.value = Math.floor((diff / (1000 * 60 * 60)) % 24)
  minutes.value = Math.floor((diff / (1000 * 60)) % 60)
  seconds.value = Math.floor((diff / 1000) % 60)

  isReady.value = true
}

onMounted(() => {
  updateCountdown()
  interval = setInterval(updateCountdown, 1000)
})

onBeforeUnmount(() => {
  clearInterval(interval)
})
</script>

<template>
  <div class="flex items-center justify-center gap-4 text-center bg-white/30 backdrop-blur-sm px-8 py-6 rounded-xl shadow-lg min-h-[120px]">
    <template v-if="isReady">
      <!-- Bloc Jours -->
      <div>
        <div class="text-4xl font-bold text-gray-800">{{ days }}</div>
        <div class="text-sm text-gray-700 uppercase tracking-wide">Jours</div>
      </div>

      <!-- Bloc Heures -->
      <div>
        <div class="text-4xl font-bold text-gray-800">{{ hours }}</div>
        <div class="text-sm text-gray-700 uppercase tracking-wide">Heures</div>
      </div>

      <!-- Bloc Minutes -->
      <div>
        <div class="text-4xl font-bold text-gray-800">{{ minutes }}</div>
        <div class="text-sm text-gray-700 uppercase tracking-wide">Minutes</div>
      </div>

      <!-- Bloc Secondes -->
      <div>
        <div class="text-4xl font-bold text-gray-800">{{ seconds }}</div>
        <div class="text-sm text-gray-700 uppercase tracking-wide">Secondes</div>
      </div>
    </template>

    <!-- ⏳ Loader -->
    <template v-else>
      <div class="w-8 h-8 border-4 border-white border-t-orange-500 rounded-full animate-spin"></div>
    </template>
  </div>
</template>
