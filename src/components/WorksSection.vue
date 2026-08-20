<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

import work1 from '../assets/works/1.mp4'
import work2 from '../assets/works/2.mp4'
import work3 from '../assets/works/3.mp4'
import work4 from '../assets/works/4.mp4'
import work5 from '../assets/works/5.mp4'
import work6 from '../assets/works/6.mp4'

const projects = [
  {
    id: 1,
    title: 'Pullin',
    category: 'UX/UI Design',
    video: work1
  },
  {
    id: 2,
    title: 'Vision(s)',
    category: 'Motion design',
    video: work2
  },
  {
    id: 3,
    title: 'Voyages E.Leclerc',
    category: 'UX/UI Design, Design system',
    video: work3
  },
  {
    id: 4,
    title: 'Interactive Design',
    category: 'Webflow & Interaction',
    video: work4
  },
  {
    id: 5,
    title: 'Motion Lab',
    category: '3D & Animation',
    video: work5
  },
  {
    id: 6,
    title: 'Custom Interfaces',
    category: 'UX/UI & Creative Direction',
    video: work6
  }
]

const videoRefs = ref([])
const cardRefs = ref([])

const playVideo = (index) => {
  const video = videoRefs.value[index]
  if (video) {
    video.play().catch(() => {})
  }
}

const pauseVideo = (index) => {
  const video = videoRefs.value[index]
  if (video) {
    video.pause()
  }
}

let observer = null

onMounted(() => {
  // Setup IntersectionObserver for auto-play on scroll into view
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        const index = cardRefs.value.indexOf(entry.target)
        if (index !== -1) {
          if (entry.isIntersecting) {
            playVideo(index)
          } else {
            pauseVideo(index)
          }
        }
      })
    },
    { threshold: 0.4 }
  )

  cardRefs.value.forEach((card) => {
    if (card) observer.observe(card)
  })
})

onBeforeUnmount(() => {
  if (observer) {
    observer.disconnect()
  }
})
</script>

<template>
  <section id="works" class="w-full px-6 md:px-10 lg:px-12 py-12 lg:py-10 select-none">
    
    <!-- Grid of Works -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 lg:gap-5">
      <div
        v-for="(project, index) in projects"
        :key="project.id"
        :ref="(el) => (cardRefs[index] = el)"
        data-cursor="view"
        class="group flex flex-col cursor-pointer"
        @mouseenter="playVideo(index)"
        @mouseleave="pauseVideo(index)"
      >
        <!-- Video / Image Container -->
        <div class="relative w-full aspect-[4/5] bg-neutral-900 overflow-hidden rounded-xs transition-transform duration-300 group-hover:scale-[1.01]">
          <video
            :ref="(el) => (videoRefs[index] = el)"
            class="w-full h-full object-cover transition-opacity duration-300"
            muted
            loop
            playsinline
            preload="auto"
          >
            <source :src="project.video" type="video/mp4" />
            Your browser does not support video.
          </video>
        </div>

        <!-- Meta Text Below -->
        <div class="mt-3.5 flex flex-col text-left">
          <h3 data-cursor="text" class="text-base md:text-lg font-medium text-black tracking-tight leading-snug">
            {{ project.title }}
          </h3>
          <p class="text-neutral-400 font-medium text-sm md:text-[15px] tracking-tight mt-0.5">
            {{ project.category }}
          </p>
        </div>

      </div>
    </div>

  </section>
</template>

<style lang="scss" scoped>
/* Smooth font rendering */
h3,
p {
  font-family: inherit;
}
</style>
