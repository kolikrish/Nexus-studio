<script setup>
import Navbar from './components/Navbar.vue'
import Hero from './components/Hero.vue'
import WorksSection from './components/WorksSection.vue'
import ExperienceSection from './components/ExperienceSection.vue'
import BrandsSection from './components/BrandsSection.vue'
import CarouselSection from './components/CarouselSection.vue'
import FooterSection from './components/FooterSection.vue'
</script>

<template>
  <div class="min-h-screen bg-white text-black font-sans flex flex-col justify-between">
    <Navbar />
    <main class="flex-grow flex flex-col justify-between">
      <Hero />
      <WorksSection />
      <ExperienceSection />
      <BrandsSection />
      <CarouselSection />
    </main>
    <FooterSection />
  </div>
</template>
