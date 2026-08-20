<script setup>
import { onMounted, onUnmounted } from 'vue'
import Lenis from 'lenis'
import 'lenis/dist/lenis.css'
import MouseFollower from './components/MouseFollower.vue'
import Navbar from './components/Navbar.vue'
import Hero from './components/Hero.vue'
import WorksSection from './components/WorksSection.vue'
import ExperienceSection from './components/ExperienceSection.vue'
import BrandsSection from './components/BrandsSection.vue'
import CarouselSection from './components/CarouselSection.vue'
import WebGLSlider from './components/WebGLSlider.vue'
import FooterSection from './components/FooterSection.vue'

let lenis = null
let rafId = null

onMounted(() => {
  lenis = new Lenis({
    duration: 2.2, // Increased smooth scroll duration for enhanced silky smooth inertia
    easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
    orientation: 'vertical',
    gestureOrientation: 'vertical',
    smoothWheel: true,
    wheelMultiplier: 1.2,
    touchMultiplier: 2.0,
    infinite: false,
  })

  window.lenis = lenis

  function raf(time) {
    lenis.raf(time)
    rafId = requestAnimationFrame(raf)
  }

  rafId = requestAnimationFrame(raf)
})

onUnmounted(() => {
  if (rafId) cancelAnimationFrame(rafId)
  if (lenis) {
    lenis.destroy()
    window.lenis = null
  }
})
</script>

<template>
  <div class="min-h-screen bg-white text-black font-sans flex flex-col justify-between">
    <MouseFollower />
    <Navbar />
    <main class="flex-grow flex flex-col justify-between">
      <Hero />
      <WorksSection />
      <ExperienceSection />
      <BrandsSection />
      <CarouselSection />
      <WebGLSlider />
    </main>
    <FooterSection />
  </div>
</template>

