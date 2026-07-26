<script setup>
import heroVideo from '../assets/Hero-video.webm'
</script>

<template>
  <section class="w-full px-6 md:px-10 lg:px-12 pt-6 lg:pt-12 pb-10 flex flex-col justify-between min-h-[calc(100vh-90px)] select-none">
    
    <!-- Top Row: Description Text (Positioned on the Right side) -->
    <div class="w-full flex justify-end mb-16 lg:mb-24">
      <div class="w-full lg:w-[62%] xl:w-[58%]">
        <p class="text-2xl sm:text-3xl md:text-4xl lg:text-[40px] xl:text-[44px] font-bold text-black leading-[1.15] tracking-tight">
          Independent designer based in Dijon, France, I create custom digital interfaces. Developing on Webflow and passionate about motion design.
        </p>
      </div>
    </div>

    <!-- Bottom Row: Selected Works Title & Video Player -->
    <div class="w-full flex flex-col lg:flex-row lg:items-end justify-between gap-8 mt-auto">
      
      <!-- Big Hero Title: Selected works* -->
      <div class="flex-1">
        <h1 class="text-[72px] sm:text-[100px] md:text-[130px] lg:text-[145px] xl:text-[175px] 2xl:text-[200px] font-extrabold text-black tracking-tighter leading-[0.84] select-text">
          Selected<br />works*
        </h1>
      </div>

      <!-- Right Bottom Video Container -->
      <div class="w-full lg:w-auto lg:min-w-[400px] xl:min-w-[480px] max-w-[560px] self-end mb-1 sm:mb-2 lg:mb-4">
        <div class="relative w-full aspect-[16/10] bg-black overflow-hidden shadow-2xl rounded-xs">
          <video
            class="w-full h-full object-cover"
            autoplay
            loop
            muted
            playsinline
            preload="auto"
          >
            <source :src="heroVideo" type="video/webm" />
            Your browser does not support the video tag.
          </video>
        </div>
      </div>

    </div>

  </section>
</template>

<style scoped>
/* Crisp rendering for display typography */
h1 {
  font-feature-settings: "kern" 1, "liga" 1;
}
</style>
