<script setup>
import { ref } from 'vue'

const navItems = ref([
  { name: 'Work,', href: '#work', active: false },
  { name: 'About,', href: '#about', active: true },
  { name: 'Playground', href: '#playground', active: false }
])
</script>

<template>
  <header class="w-full px-6 py-5 md:px-10 md:py-6 lg:px-12 flex justify-between items-center select-none text-sm md:text-base font-sans tracking-tight">
    <!-- Left: Brand / Name -->
    <div class="font-bold text-black tracking-tight text-base md:text-lg">
      <a href="/" class="hover:opacity-80 transition-opacity">Julien Pianetti</a>
    </div>

    <!-- Center: Navigation Links -->
    <nav class="flex items-center gap-1.5 md:gap-2 text-base md:text-[17px] font-semibold">
      <a
        v-for="(item, index) in navItems"
        :key="index"
        :href="item.href"
        :class="[
          item.active || item.name !== 'Work,'
            ? 'text-black font-bold'
            : 'text-neutral-400 font-semibold hover:text-black',
          'transition-colors duration-200 cursor-pointer'
        ]"
      >
        {{ item.name }}
      </a>
    </nav>

    <!-- Right: Contact Email -->
    <div class="text-base md:text-[17px] font-bold text-black tracking-tight">
      <a href="mailto:jupian@gmail.com" class="hover:opacity-75 transition-opacity">jupian@gmail.com</a>
    </div>
  </header>
</template>
