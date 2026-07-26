<script setup>
import Navbar from './components/Navbar.vue'
import Hero from './components/Hero.vue'
</script>

<template>
  <div class="min-h-screen bg-white text-black font-sans flex flex-col justify-between">
    <Navbar />
    <main class="flex-grow flex flex-col justify-between">
      <Hero />
    </main>
  </div>
</template>
