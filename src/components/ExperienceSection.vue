<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import gsap from 'gsap'

const statementRef = ref(null)
const hasAnimated = ref(false)

const statementText = "After 14 years of shaping web experiences, I've expanded my playground: from interface design to Webflow development, all the way to animation that brings projects to life."
const statementWords = statementText.split(" ")

let observer = null

onMounted(() => {
  if (!statementRef.value) return

  const wordElements = statementRef.value.querySelectorAll('.word-inner')

  // Set initial hidden state
  gsap.set(wordElements, { y: '-100%', opacity: 0 })

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting && !hasAnimated.value) {
          hasAnimated.value = true
          gsap.to(wordElements, {
            y: '0%',
            opacity: 1,
            duration: 0.6,
            stagger: 0.03,
            ease: 'power3.out',
          })
          observer.disconnect()
        }
      })
    },
    { threshold: 0.2 }
  )

  observer.observe(statementRef.value)
})

onBeforeUnmount(() => {
  if (observer) observer.disconnect()
})

const experiences = [
  {
    number: '01',
    title: 'Digital Design',
    services: [
      'UX / UI Design',
      'Art direction',
      'Design system',
      'E-commerce',
      'Documentation'
    ],
    type: 'shapes'
  },
  {
    number: '02',
    title: 'Webflow',
    services: [
      'Expert Webflow',
      'CMS & Localization',
      'Client First',
      'Landing pages',
      'Claude code'
    ],
    type: 'webflow'
  },
  {
    number: '03',
    title: 'Motion Design',
    services: [
      'Logo animation',
      'Interface animation',
      'Showreel',
      'Advertisement',
      'Expérimentations'
    ],
    type: 'motion'
  }
]
</script>

<template>
  <section id="experience" class="w-full px-6 md:px-10 lg:px-12 py-16 lg:py-20 select-none">
    
    <!-- Main Headline Statement -->
    <div class="w-full max-w-7xl mb-16 lg:mb-20">
      <h2
        ref="statementRef"
        data-cursor="text"
        class="text-3xl sm:text-4xl md:text-5xl lg:text-[48px] xl:text-[40px] font-[poppins] text-black leading-[1.12] tracking-tight flex flex-wrap gap-x-[0.28em] gap-y-1.5"
      >
        <span
          v-for="(word, index) in statementWords"
          :key="index"
          class="inline-block overflow-hidden py-1 -my-1"
        >
          <span class="word-inner inline-block">
            {{ word }}
          </span>
        </span>
      </h2>
    </div>

    <!-- 3 Experience Cards Grid -->
    <div class="grid grid-cols-1 md:grid-cols-3 border-t border-neutral-200">
      <div
        v-for="(exp, index) in experiences"
        :key="exp.number"
        :class="[
          'flex flex-col justify-between p-6 sm:p-8 lg:p-10 min-h-[460px] lg:min-h-[420px]',
          index < experiences.length - 1 ? 'border-b md:border-b-0 md:border-r border-neutral-200' : ''
        ]"
      >
        <!-- Top Portion: Number & Content -->
        <div>
          <!-- Bullet & Number -->
          <div class="flex items-center gap-2 text-xs md:text-sm font-light tracking-widest mb-10 lg:mb-14">
            <span class="w-1.5 h-1.5 rounded-full bg-black inline-block"></span>
            <span>{{ exp.number }}</span>
          </div>

          <!-- Section Title -->
          <h3 class="text-3xl md:text-4xl lg:text-[30px] font-medium tracking-tight mb-6 leading-tight">
            {{ exp.title }}
          </h3>

          <!-- Services Bullet List -->
          <ul class="text-sm md:text-sm font-light text-neutral-800 tracking-tight">
            <li v-for="(service, sIdx) in exp.services" :key="sIdx">
              {{ service }}
            </li>
          </ul>
        </div>

        <!-- Bottom Portion: Custom SVG Graphics -->
        <div class="pt-12">
          
          <!-- Graphic 1: Square + Triangle -->
          <svg v-if="exp.type === 'shapes'" class="w-18 h-14 text-black" viewBox="0 0 100 80" fill="currentColor">
            <rect x="0" y="30" width="36" height="36" fill="black" />
            <polygon points="45,66 90,66 67.5,22" fill="black" />
          </svg>

          <!-- Graphic 2: Webflow Layout Grid -->
          <svg v-else-if="exp.type === 'webflow'" class="w-18 h-14 text-black" viewBox="0 0 100 80" fill="currentColor">
            <rect x="0" y="0" width="70" height="24" fill="black" />
            <rect x="0" y="30" width="32" height="42" fill="black" />
            <rect x="38" y="30" width="32" height="42" fill="black" />
          </svg>

          <!-- Graphic 3: Motion Chevrons -->
          <svg v-else-if="exp.type === 'motion'" class="w-20 h-14 text-black" viewBox="0 0 120 70" fill="currentColor">
            <polygon points="0,5 20,35 0,65" fill="black" />
            <polygon points="22,5 42,35 22,65" fill="black" />
            <polygon points="44,5 64,35 44,65" fill="black" />
            <polygon points="66,5 86,35 66,65" fill="black" />
          </svg>

        </div>

      </div>
    </div>

  </section>
</template>
