<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

import work1 from '../assets/works/1.mp4'
import work2 from '../assets/works/2.mp4'
import work3 from '../assets/works/3.mp4'
import work4 from '../assets/works/4.mp4'
import work5 from '../assets/works/5.mp4'
import work6 from '../assets/works/6.mp4'

const originalItems = [
  {
    id: 1,
    title: 'Volte',
    video: work1,
    aspect: 'aspect-square',
    width: 'w-[320px] sm:w-[380px] lg:w-[420px]'
  },
  {
    id: 2,
    title: '11',
    video: work2,
    aspect: 'aspect-[16/10]',
    width: 'w-[380px] sm:w-[440px] lg:w-[480px]'
  },
  {
    id: 3,
    title: 'RunWise motion',
    video: work3,
    aspect: 'aspect-square',
    width: 'w-[320px] sm:w-[380px] lg:w-[420px]'
  },
  {
    id: 4,
    title: 'Oblique doc',
    video: work4,
    aspect: 'aspect-[16/10]',
    width: 'w-[380px] sm:w-[440px] lg:w-[480px]'
  },
  {
    id: 5,
    title: 'Poster animation',
    video: work5,
    aspect: 'aspect-[3/4]',
    width: 'w-[300px] sm:w-[360px] lg:w-[400px]'
  },
  {
    id: 6,
    title: 'Motion Exploration',
    video: work6,
    aspect: 'aspect-square',
    width: 'w-[320px] sm:w-[380px] lg:w-[420px]'
  }
]

// Duplicate array for seamless infinite looping
const items = computed(() => [
  ...originalItems.map(i => ({ ...i, id: `${i.id}-a` })),
  ...originalItems.map(i => ({ ...i, id: `${i.id}-b` }))
])

const scrollContainer = ref(null)
const isPaused = ref(false)

let isDown = false
let startX = 0
let scrollLeft = 0
let animationFrameId = null
const scrollSpeed = 4 // Pixels per frame for smooth scrolling

const autoScroll = () => {
  if (scrollContainer.value && !isPaused.value && !isDown) {
    scrollContainer.value.scrollLeft += scrollSpeed

    // Reset scroll positions when half-way through duplicated set
    const maxScroll = scrollContainer.value.scrollWidth / 2
    if (scrollContainer.value.scrollLeft >= maxScroll) {
      scrollContainer.value.scrollLeft -= maxScroll
    }
  }
  animationFrameId = requestAnimationFrame(autoScroll)
}

onMounted(() => {
  animationFrameId = requestAnimationFrame(autoScroll)
})

onBeforeUnmount(() => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId)
  }
})

// Mouse Drag-to-Scroll Handlers
const startDragging = (e) => {
  isDown = true
  isPaused.value = true
  startX = e.pageX - scrollContainer.value.offsetLeft
  scrollLeft = scrollContainer.value.scrollLeft
}

const stopDragging = () => {
  isDown = false
  isPaused.value = false
}

const moveDragging = (e) => {
  if (!isDown) return
  e.preventDefault()
  const x = e.pageX - scrollContainer.value.offsetLeft
  const walk = (x - startX) * 1.5
  scrollContainer.value.scrollLeft = scrollLeft - walk
}
</script>

<template>
  <section id="carousel" class="w-full py-12 lg:py-10 select-none overflow-hidden">
    
    <!-- Automatic Infinite Scrollable Track -->
    <div
      ref="scrollContainer"
      data-cursor="drag"
      class="flex gap-6 lg:gap-8 overflow-x-auto scrollbar-none px-6 md:px-10 lg:px-12 py-4 cursor-grab active:cursor-grabbing"
      @mouseleave="stopDragging"
      @mousedown="startDragging"
      @mouseup="stopDragging"
      @mousemove="moveDragging"
    >
      <div
        v-for="item in items"
        :key="item.id"
        :class="['flex-none flex flex-col group', item.width]"
      >
        <!-- Video Card Wrapper -->
        <div
          data-cursor="view"
          :class="['relative w-full bg-black overflow-hidden rounded-xs shadow-md transition-transform duration-300 group-hover:scale-[1.01]', item.aspect]"
        >
          <video
            class="w-full h-full object-cover pointer-events-none"
            autoplay
            loop
            muted
            playsinline
            preload="auto"
          >
            <source :src="item.video" type="video/mp4" />
            Your browser does not support video playback.
          </video>
        </div>

        <!-- Title Below Card -->
        <div class="mt-3.5 text-left">
          <p data-cursor="text" class="text-sm md:text-base font-light text-black tracking-tight font-[poppins]">
            {{ item.title }}
          </p>
        </div>

      </div>
    </div>

  </section>
</template>

<style lang="scss" scoped>

/* Hide scrollbar across browsers */
.scrollbar-none::-webkit-scrollbar {
  display: none;
}
.scrollbar-none {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

</style>
