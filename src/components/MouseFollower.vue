<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const follower = ref(null)

const isVisible = ref(false)
const isHovered = ref(false)
const isClicked = ref(false)
const hoverType = ref('') // 'text', 'media', 'drag', 'button', etc.
const cursorText = ref('')

// Target and current positions for lerp animation
let mouseX = -100
let mouseY = -100
let currentX = -100
let currentY = -100
let rafId = null

const lerp = (start, end, factor) => start + (end - start) * factor

const updatePosition = () => {
  currentX = lerp(currentX, mouseX, 0.16)
  currentY = lerp(currentY, mouseY, 0.16)

  if (follower.value) {
    follower.value.style.transform = `translate3d(${currentX}px, ${currentY}px, 0)`
  }

  rafId = requestAnimationFrame(updatePosition)
}

const onMouseMove = (e) => {
  mouseX = e.clientX
  mouseY = e.clientY

  if (!isVisible.value) {
    currentX = mouseX
    currentY = mouseY
    isVisible.value = true
  }

  const target = e.target
  if (!target) return

  // Find nearest interactive or annotated hover element
  const cursorEl = target.closest('[data-cursor], [data-cursor-text], a, button, h1, h2, h3, video, img, .group')

  if (cursorEl) {
    isHovered.value = true

    const customType = cursorEl.getAttribute('data-cursor')
    const customText = cursorEl.getAttribute('data-cursor-text')

    if (customText) {
      cursorText.value = customText
    } else if (customType === 'view' || customType === 'media') {
      cursorText.value = 'VIEW'
    } else if (customType === 'drag') {
      cursorText.value = 'DRAG'
    } else {
      cursorText.value = ''
    }

    if (customType) {
      hoverType.value = customType
    } else if (cursorEl.tagName === 'VIDEO' || cursorEl.tagName === 'IMG' || cursorEl.querySelector('video, img')) {
      hoverType.value = 'media'
      if (!cursorText.value) cursorText.value = 'VIEW'
    } else if (['H1', 'H2', 'H3'].includes(cursorEl.tagName)) {
      hoverType.value = 'text'
    } else if (cursorEl.tagName === 'A' || cursorEl.tagName === 'BUTTON') {
      hoverType.value = 'button'
    } else {
      hoverType.value = 'default-hover'
    }
  } else {
    isHovered.value = false
    hoverType.value = ''
    cursorText.value = ''
  }
}

const onMouseDown = () => {
  isClicked.value = true
}

const onMouseUp = () => {
  isClicked.value = false
}

const onMouseLeave = () => {
  isVisible.value = false
}

const onMouseEnter = () => {
  isVisible.value = true
}

onMounted(() => {
  // Only initialize mouse follower on desktop/fine-pointer devices
  if (window.matchMedia('(pointer: fine)').matches) {
    window.addEventListener('mousemove', onMouseMove, { passive: true })
    window.addEventListener('mousedown', onMouseDown, { passive: true })
    window.addEventListener('mouseup', onMouseUp, { passive: true })
    document.addEventListener('mouseleave', onMouseLeave, { passive: true })
    document.addEventListener('mouseenter', onMouseEnter, { passive: true })

    rafId = requestAnimationFrame(updatePosition)
  }
})

onUnmounted(() => {
  window.removeEventListener('mousemove', onMouseMove)
  window.removeEventListener('mousedown', onMouseDown)
  window.removeEventListener('mouseup', onMouseUp)
  document.removeEventListener('mouseleave', onMouseLeave)
  document.removeEventListener('mouseenter', onMouseEnter)
  if (rafId) cancelAnimationFrame(rafId)
})
</script>

<template>
  <div
    ref="follower"
    class="pointer-events-none fixed top-0 left-0 z-[9999] -translate-x-1/2 -translate-y-1/2 transition-opacity duration-300 hidden md:block"
    :class="[
      isVisible ? 'opacity-100' : 'opacity-0'
    ]"
  >
    <!-- Scalable Circle Follower -->
    <div
      class="relative flex items-center justify-center rounded-full transition-all duration-300 cubic-bezier(0.16, 1, 0.3, 1)"
      :class="[
        isClicked ? 'scale-85' : 'scale-100',
        
        // 1. Default non-hover state: Minimal smooth circle with center dot
        !isHovered ? 'w-8 h-8 border border-black/30 bg-black/5' : '',

        // 2. Hover state on Images & Video cards: Scales up significantly to a bold dark circle with 'VIEW' text
        isHovered && (hoverType === 'media' || hoverType === 'view')
          ? 'w-24 h-24 bg-black text-white shadow-2xl scale-100'
          : '',

        // 3. Hover state on Headings / Specific texts: Scales circle with mix-blend-difference contrast inversion
        isHovered && hoverType === 'text'
          ? 'w-20 h-20 bg-black text-white mix-blend-difference scale-100'
          : '',

        // 4. Hover state on Carousel track: Scales circle with 'DRAG' indicator
        isHovered && hoverType === 'drag'
          ? 'w-20 h-20 bg-black text-white scale-100 shadow-xl'
          : '',

        // 5. Hover state on Links & Buttons: Scales up to pill/circle
        isHovered && hoverType === 'button'
          ? 'w-12 h-12 bg-black text-white scale-100 shadow-md'
          : '',

        // 6. Generic hover fallback: Outer scaled ring
        isHovered && hoverType === 'default-hover'
          ? 'w-16 h-16 border-2 border-black/80 bg-black/10 scale-100'
          : '',
      ]"
    >
      <!-- Center Dot when resting -->
      <span
        v-if="!isHovered"
        class="w-1.5 h-1.5 rounded-full bg-black transition-transform duration-200"
      ></span>

      <!-- Text Label inside Circle when hovering specified elements -->
      <span
        v-if="isHovered && cursorText"
        class="text-[11px] font-semibold tracking-widest uppercase font-[poppins] animate-pop-in select-none"
      >
        {{ cursorText }}
      </span>
    </div>
  </div>
</template>

<style scoped>
@keyframes popIn {
  from {
    opacity: 0;
    transform: scale(0.6);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.animate-pop-in {
  animation: popIn 0.22s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}
</style>
