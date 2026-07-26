<script setup>
const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}
</script>

<template>
  <footer class="w-full px-6 md:px-10 lg:px-12 pt-16 lg:pt-20 pb-10 bg-white text-black select-none">
    
    <!-- Big Headline -->
    <div class="mb-20 lg:mb-18">
      <h1 class="text-[64px] sm:text-[100px] md:text-[140px] lg:text-[170px] xl:text-[200px] tracking-tight leading-[0.85] font-[poppins]">
        Let's<br />Collaborate!
      </h1>
    </div>

    <!-- Divider Line -->
    <div class="border-t border-neutral-200 pt-10 lg:pt-10 mb-10">
      <!-- 3 Columns Grid -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-10 lg:gap-12">
        
        <!-- Column 1: MENU -->
        <div class="flex flex-col">
          <p class="text-xs font-light text-neutral-400 uppercase mb-6 font-[poppins]">
            MENU
          </p>
          <ul class="space-y-3.5 text-base md:text-lg font-medium text-black font-[poppins]">
            <li>
              <a href="#" class="hover:opacity-60 transition-opacity">Home</a>
            </li>
            <li>
              <a href="#works" class="hover:opacity-60 transition-opacity">Work</a>
            </li>
            <li>
              <a href="#carousel" class="hover:opacity-60 transition-opacity">Playground</a>
            </li>
            <li>
              <a href="#experience" class="hover:opacity-60 transition-opacity">About</a>
            </li>
          </ul>
        </div>

        <!-- Column 2: CONNECT -->
        <div class="flex flex-col">
          <p class="text-xs font-light text-neutral-400 tracking-widest uppercase mb-6 font-[poppins]">
            CONNECT
          </p>
          <ul class="space-y-3.5 text-base md:text-lg font-medium text-black font-[poppins]">
            <li>
              <a href="https://linkedin.com" target="_blank" class="hover:opacity-60 transition-opacity inline-flex items-center gap-1">
                LinkedIn <span class="text-sm">↗</span>
              </a>
            </li>
            <li>
              <a href="mailto:jupian@gmail.com" class="hover:opacity-60 transition-opacity">Email</a>
            </li>
            <li>
              <a href="#" class="hover:opacity-60 transition-opacity inline-flex items-center gap-1">
                Resources <span class="text-sm">↗</span>
              </a>
            </li>
          </ul>
        </div>

        <!-- Column 3: SAY HELLO -->
        <div class="flex flex-col">
          <p class="text-xs font-light text-neutral-400 tracking-widest uppercase mb-6 font-[poppins]">
            SAY HELLO
          </p>
          <a
            href="mailto:jupian@gmail.com"
            class="text-2xl sm:text-3xl lg:text-[38px] font-medium text-black tracking-tight hover:opacity-60 transition-opacity block mb-4 font-[poppins]"
          >
            jupian@gmail.com
          </a>

          <!-- Status Indicator -->
          <div class="flex items-center gap-2.5 text-sm md:text-base font-light text-neutral-600 font-[poppins]">
            <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 inline-block animate-pulse"></span>
            <span>Available for work — Dijon, France</span>
          </div>
        </div>

      </div>
    </div>

    <!-- Bottom Footer Bar -->
    <div class="border-t border-neutral-200 pt-8 flex flex-col sm:flex-row justify-between items-center gap-4 text-xs md:text-sm font-light text-neutral-500 tracking-tight font-[poppins]">
      <div>
        ©2026 Julien Pianetti — All rights reserved
      </div>
      <button
        @click="scrollToTop"
        class="hover:text-black transition-colors cursor-pointer uppercase font-medium flex items-center gap-1 tracking-wider"
      >
        BACK TO TOP
      </button>
    </div>

  </footer>
</template>
