<template>
  <div class="min-h-screen bg-gradient-to-br from-amber-200 via-orange-300 to-red-400 flex flex-col items-center justify-center px-4">
    <LeafCatcher />

    <!-- 📸 Bloc photos avec animation -->
    <div class="relative w-full max-w-6xl h-[500px] mx-auto md:mt-16 mb-3">
      <!-- Photo gauche -->
      <NuxtImg
        src="/img/asterix_fixed.jpg"
        width="250"
        height="300"
        alt="Photo couple parc"
        class="absolute md:block hidden top-0 left-12 -rotate-2 z-10 rounded-xl shadow-2xl ring-4 ring-white transition-transform duration-300 hover:scale-105 transition-all duration-1000 ease-out"
        :class="showPhotos ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4'"
      />

      <!-- Photo centrale -->
      <NuxtImg
        src="/img/bague_fixed.jpg"
        width="300"
        height="350"
        alt="Photo bague"
        class="absolute top-24 left-1/2 -translate-x-1/2 -rotate-1 z-20 rounded-xl shadow-2xl ring-4 ring-white transition-transform duration-300 hover:scale-105 transition-all duration-1000 delay-200 ease-out"
        :class="showPhotos ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4'"
      />

      <!-- Photo droite -->
      <NuxtImg
        src="/img/maison_fixed.jpg"
        width="250"
        height="300"
        alt="Photo bisou"
        class="absolute md:block hidden top-0 right-12 -rotate-2 z-10 rounded-xl shadow-2xl ring-4 ring-white transition-transform duration-300 hover:scale-105 transition-all duration-1000 delay-400 ease-out"
        :class="showPhotos ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4'"
      />
    </div>

    <!-- 🧡 Titre -->
    <h1 class="text-4xl md:text-5xl font-extrabold text-center text-gray-800 leading-tight mb-8">
      Vous êtes invité au
      <span class="text-orange-600 drop-shadow-md">mariage</span>
      de
      <br>
      <span class="text-orange-600 drop-shadow-md">👰🏻‍♀️ Rosanna</span>
      et
      <span class="text-orange-600 drop-shadow-md">🤵🏻 Aymeric</span>
      <br>
      le
      <span class="text-orange-600 drop-shadow-md">24 octobre 2026</span>
    </h1>

    <!-- 🕰️ Sous-titre -->
    <h2 class="text-xl md:text-xl font-light italic">
      Le mariage commence dans :
    </h2>

    <!-- ⏳ Compte à rebours -->
    <div class="text-center px-6 py-4 text-xl md:text-2xl font-semibold text-gray-700">
      <CountDown />
    </div>

    <!-- ℹ️ Message -->
    <p class="text-center text-sm text-gray-800 mt-4">
      Informations complémentaires à venir...
    </p>

    <!-- 🔒 Contenu masqué pour plus tard -->
    <div class="hidden">
      <button
        class="mb-4 px-6 py-2 rounded-full bg-white/60 text-orange-700 font-medium shadow hover:bg-white/80 transition"
      >
        Voir la date du mariage
      </button>

      <div class="bg-white/40 backdrop-blur-sm px-5 py-3 rounded-lg shadow-md text-gray-800 font-light text-lg text-center max-w-md">
        📅 Le mariage aura lieu le week-end du <span class="font-semibold">24 octobre 2026</span>.
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'

const showPhotos = ref(false)

onMounted(() => {
  nextTick(() => {
    showPhotos.value = true
  })
})
</script>
