<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import gsap from 'gsap'

const titleRef = ref(null)
const hasTitleAnimated = ref(false)

const titleText = "I've had the chance to work with these brands"
const titleWords = titleText.split(" ")

let titleObserver = null

onMounted(() => {
  if (!titleRef.value) return

  const wordElements = titleRef.value.querySelectorAll('.brand-title-word-inner')

  gsap.set(wordElements, { y: '-100%', opacity: 0 })

  titleObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting && !hasTitleAnimated.value) {
          hasTitleAnimated.value = true
          gsap.to(wordElements, {
            y: '0%',
            opacity: 1,
            duration: 0.6,
            stagger: 0.035,
            ease: 'power3.out',
          })
          titleObserver.disconnect()
        }
      })
    },
    { threshold: 0.2 }
  )

  titleObserver.observe(titleRef.value)
})

onBeforeUnmount(() => {
  if (titleObserver) titleObserver.disconnect()
})

const brands = [
  { name: 'Picture Organic Clothing', image: 'https://images.unsplash.com/photo-1551028719-00167b16eac5?q=80&w=600&auto=format&fit=crop' },
  { name: 'Podcast Vision(s)', image: 'https://images.unsplash.com/photo-1590602847861-f357a9332bbc?q=80&w=600&auto=format&fit=crop' },
  { name: 'Pullin', image: 'https://images.unsplash.com/photo-1576995853123-5a10305d93c0?q=80&w=600&auto=format&fit=crop' },
  { name: 'Voyages E. Leclerc', image: 'https://images.unsplash.com/photo-1436491865332-7a61a109cc05?q=80&w=600&auto=format&fit=crop' },
  { name: 'Aliocha Boi', image: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=600&auto=format&fit=crop' },
  { name: 'LG Seeds', image: 'https://images.unsplash.com/photo-1500382017468-9049fed747ef?q=80&w=600&auto=format&fit=crop' },
  { name: 'Limagrain', image: 'https://images.unsplash.com/photo-1500937386664-56d1dfef3854?q=80&w=600&auto=format&fit=crop' },
  { name: 'Nöliö', image: 'https://images.unsplash.com/photo-1461896836934-ffe607ba8211?q=80&w=600&auto=format&fit=crop' },
  { name: 'Hindbag', image: 'https://images.unsplash.com/photo-1548036328-c9fa89d128fa?q=80&w=600&auto=format&fit=crop' },
  { name: 'Muule', image: 'https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?q=80&w=600&auto=format&fit=crop' },
  { name: 'La Rosée', image: 'https://images.unsplash.com/photo-1556228720-195a672e8a03?q=80&w=600&auto=format&fit=crop' },
  { name: 'Campsider', image: 'https://images.unsplash.com/photo-1504280390367-361c6d9f38f4?q=80&w=600&auto=format&fit=crop' },
  { name: 'Henri Duvillard', image: 'https://images.unsplash.com/photo-1551698618-1dfe5d97d256?q=80&w=600&auto=format&fit=crop' },
  { name: 'Eider', image: 'https://images.unsplash.com/photo-1542291026-7eec264c27ff?q=80&w=600&auto=format&fit=crop' },
  { name: 'Café Lomi', image: 'https://images.unsplash.com/photo-1501339847302-ac426a4a7cbb?q=80&w=600&auto=format&fit=crop' },
  { name: 'Volkswagen Group', image: 'https://images.unsplash.com/photo-1617788138017-80ad40651399?q=80&w=600&auto=format&fit=crop' },
  { name: 'HVC Premium', image: 'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?q=80&w=600&auto=format&fit=crop' },
  { name: 'Fidroit', image: 'https://images.unsplash.com/photo-1486406146926-c627a92ad1ab?q=80&w=600&auto=format&fit=crop' },
  { name: 'Kraaft', image: 'https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?q=80&w=600&auto=format&fit=crop' },
  { name: 'Aubrilam', image: 'https://images.unsplash.com/photo-1513694203232-719a280e022f?q=80&w=600&auto=format&fit=crop' },
  { name: 'Snowleader', image: 'https://images.unsplash.com/photo-1517048676732-d65bc937f952?q=80&w=600&auto=format&fit=crop' },
  { name: 'Horse Pilot', image: 'https://images.unsplash.com/photo-1553284965-83fd3e82fa5a?q=80&w=600&auto=format&fit=crop' },
  { name: 'Arkea BnB', image: 'https://images.unsplash.com/photo-1566073771259-6a8506099945?q=80&w=600&auto=format&fit=crop' },
  { name: 'Mavala', image: 'https://images.unsplash.com/photo-1522337360788-8b13dee7a37e?q=80&w=600&auto=format&fit=crop' },
  { name: 'Minimiil', image: 'https://images.unsplash.com/photo-1526170375885-4d8ecf77b99f?q=80&w=600&auto=format&fit=crop' },
  { name: 'Alvadiem', image: 'https://images.unsplash.com/photo-1608248597263-0057e43a4524?q=80&w=600&auto=format&fit=crop' },
  { name: 'Jaws', image: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?q=80&w=600&auto=format&fit=crop' },
  { name: 'Sturia', image: 'https://images.unsplash.com/photo-1534422298391-e4f8c172dddb?q=80&w=600&auto=format&fit=crop' },
  { name: 'ACE Hôtels', image: 'https://images.unsplash.com/photo-1566073771259-6a8506099945?q=80&w=600&auto=format&fit=crop' },
  { name: 'Parlez-moi de Paris', image: 'https://images.unsplash.com/photo-1502602898657-3e91760cbb34?q=80&w=600&auto=format&fit=crop' },
  { name: 'Bamo Experience', image: 'https://images.unsplash.com/photo-1512917774080-9991f1c4c750?q=80&w=600&auto=format&fit=crop' },
  { name: 'Michelin', image: 'https://images.unsplash.com/photo-1558981403-c5f9899a28bc?q=80&w=600&auto=format&fit=crop' },
  { name: 'Belle Allure', image: 'https://images.unsplash.com/photo-1490481651871-ab68de25d43d?q=80&w=600&auto=format&fit=crop' },
  { name: 'Arva', image: 'https://images.unsplash.com/photo-1486870591958-9b9d0d1dda99?q=80&w=600&auto=format&fit=crop' },
  { name: 'Easy Snowboards', image: 'https://images.unsplash.com/photo-1565992441121-4367c2967103?q=80&w=600&auto=format&fit=crop' },
  { name: 'Gravel Up', image: 'https://images.unsplash.com/photo-1485965120184-e220f721d03e?q=80&w=600&auto=format&fit=crop' },
  { name: 'Hervé', image: 'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?q=80&w=600&auto=format&fit=crop' }
]

const activeIndex = ref(null)
const mousePos = ref({ x: 0, y: 0 })

const handleMouseMove = (e) => {
  mousePos.value = {
    x: e.clientX,
    y: e.clientY
  }
}

const setActive = (idx) => {
  activeIndex.value = idx
}

const clearActive = () => {
  activeIndex.value = null
}
</script>

<template>
  <section
    id="brands"
    class="w-full px-6 md:px-10 lg:px-12 py-16 lg:py-20 select-none relative"
    @mousemove="handleMouseMove"
  >
    <!-- Section Title -->
    <h2
      ref="titleRef"
      data-cursor="text"
      class="text-3xl sm:text-4xl md:text-5xl lg:text-[52px] font-[poppins] text-black tracking-tight mb-12 lg:mb-16 flex flex-wrap gap-x-[0.28em] gap-y-1.5"
    >
      <span
        v-for="(word, index) in titleWords"
        :key="index"
        class="inline-block overflow-hidden py-1 -my-1"
      >
        <span class="brand-title-word-inner inline-block">
          {{ word }}
        </span>
      </span>
    </h2>

    <!-- Brands Paragraph / Grid -->
    <div class="w-full flex flex-wrap items-center gap-y-3 leading-[1.25] text-3xl sm:text-4xl md:text-5xl lg:text-[52px] xl:text-[42px] font-[poppins]">
      <template v-for="(brand, idx) in brands" :key="idx">
        <span
          class="transition-colors duration-300 cursor-pointer"
          :class="[
            activeIndex !== null && activeIndex !== idx
              ? 'text-neutral-300'
              : 'text-black'
          ]"
          @mouseenter="setActive(idx)"
          @mouseleave="clearActive"
        >
          {{ brand.name }}
        </span>

        <!-- Bullet Separator -->
        <span
          v-if="idx < brands.length - 1"
          class="mx-2 md:mx-3 transition-colors duration-300"
          :class="[
            activeIndex !== null ? 'text-neutral-300' : 'text-black'
          ]"
        >
          ·
        </span>
      </template>
    </div>

    <!-- Floating Cursor Preview Image -->
    <div
      v-if="activeIndex !== null"
      class="fixed pointer-events-none z-50 transition-transform duration-100 ease-out -translate-x-1/2 -translate-y-1/1"
      :style="{
        left: `${mousePos.x}px`,
        top: `${mousePos.y - 40}px`
      }"
    >
      <div class="w-54 h-34 sm:w-72 sm:h-48 md:w-80 md:h-52 bg-black rounded-sm overflow-hidden">
        <img
          :src="brands[activeIndex].image"
          :alt="brands[activeIndex].name"
          class="w-full h-full object-cover"
        />
      </div>
    </div>

  </section>
</template>


<style lang="scss" scoped>

span {
  display: inline-block;
}

</style>