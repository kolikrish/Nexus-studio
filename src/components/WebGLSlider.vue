<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import * as THREE from 'three'
import gsap from 'gsap'
import SplitType from 'split-type'

const slides = [
  {
    title: "Blackwater '91",
    description: "Flickering lanterns and twisted masks welcome unwanted visitors into a strange celebration beyond the forest trail.",
    image: "/assets/slider/slider-img-1.jpg"
  },
  {
    title: "Crimson Theory",
    description: "A mysterious performer slowly loses reality beneath violent lights and unsettling mirrored reflections inside an empty theater.",
    image: "/assets/slider/slider-img-2.jpg"
  },
  {
    title: "Tape Delay Archives",
    description: "Stacks of dusty videotapes and glowing static fill the room during another endless night without a single moment of sleep.",
    image: "/assets/slider/slider-img-3.jpg"
  },
  {
    title: "Exit 14 Westbound",
    description: "An endless stretch of cracked pavement disappears into the heavy fog, where the hum of the engine is the only proof that time hasn't stopped completely.",
    image: "/assets/slider/slider-img-4.jpg"
  },
  {
    title: "The Last Mile",
    description: "Faded billboards and rusted signs point toward a destination that no longer exists on any modern map, leaving only ghosts behind in the rearview mirror.",
    image: "/assets/slider/slider-img-5.jpg"
  }
]

const sliderRef = ref(null)
const slideContentRef = ref(null)

const currentIndex = ref(0)
const isTransitioning = ref(false)

let scene, camera, renderer, material, plane
let textures = []
let rippleTween = null
let animationFrameId = null

// GLSL Shaders
const vertexShader = `
varying vec2 vUv;

void main() {
    vUv = uv;
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}
`

const fragmentShader = `
uniform sampler2D uTexCurrent;
uniform sampler2D uTexNext;
uniform float uProgress;
uniform vec2 uResolution;
uniform vec2 uTexCurrentRes;
uniform vec2 uTexNextRes;
uniform float uWaveFreq;
uniform float uWavePow;
uniform float uWaveWidth;
uniform float uFalloff;
uniform float uBoostStrength;
uniform float uCrossfadeWidth;
uniform float uMobile;

varying vec2 vUv;

vec2 getImageUv(vec2 uv, vec2 screenRes, vec2 imgRes, vec2 boxMin, vec2 boxMax) {
    vec2 boxUv = (uv - boxMin) / (boxMax - boxMin);
    vec2 boxSize = (boxMax - boxMin) * screenRes;
    float boxAspect = boxSize.x / boxSize.y;
    float imgAspect = imgRes.x / imgRes.y;

    vec2 scale = vec2(1.0);
    if (boxAspect > imgAspect) {
        scale.y = imgAspect / boxAspect;
    } else {
        scale.x = boxAspect / imgAspect;
    }

    return (boxUv - 0.5) * scale + 0.5;
}

bool isInsideBox(vec2 uv, vec2 boxMin, vec2 boxMax) {
    return uv.x >= boxMin.x && uv.x <= boxMax.x && uv.y >= boxMin.y && uv.y <= boxMax.y;
}

void main() {
    vec2 boxMin = mix(vec2(0.25, 0.175), vec2(0.0), uMobile);
    vec2 boxMax = mix(vec2(0.75, 0.825), vec2(1.0), uMobile);

    float aspectRatio = uResolution.y / uResolution.x;
    vec2 coord = vec2(vUv.x, vUv.y * aspectRatio);
    vec2 center = vec2(0.5, 0.5 * aspectRatio);

    float dist = distance(coord, center);
    float time = uProgress;

    vec2 displaced = coord;
    float brightness = 0.0;
    float blend = 0.0;

    if (time > 0.001) {
        float trailing = dist - time;

        if (trailing < uWaveWidth && trailing < 0.0) {
            float age = -trailing;
            float decay = exp(-age * uFalloff);
            float wave = sin(age * uWaveFreq) * decay;

            vec2 direction = normalize(coord - center);
            displaced += direction * wave * uWavePow;
            brightness = abs(wave) * uBoostStrength * decay;
        }

        blend = smoothstep(0.0, uCrossfadeWidth, -trailing);
    }

    vec2 finalUv = vec2(displaced.x, displaced.y / aspectRatio);
    
    vec2 imageUvCurrent = getImageUv(finalUv, uResolution, uTexCurrentRes, boxMin, boxMax);
    vec2 imageUvNext = getImageUv(finalUv, uResolution, uTexNextRes, boxMin, boxMax);

    vec4 currentColor = texture2D(uTexCurrent, imageUvCurrent);
    vec4 nextColor = texture2D(uTexNext, imageUvNext);

    vec4 color = mix(currentColor, nextColor, blend);
    color.rgb += color.rgb * brightness;

    if (!isInsideBox(finalUv, boxMin, boxMax)) {
        color = vec4(0.0);
    }

    gl_FragColor = color;
}
`

const rippleConfig = {
  waveFreq: 25.0,
  wavePow: 0.035,
  waveWidth: 0.5,
  falloff: 10.0,
  boostStrength: 0.5,
  crossfadeWidth: 0.05,
  duration: 3.0,
  endValue: 1.0,
  ease: "power2.out",
}

const uniforms = {
  uTexCurrentRes: { value: new THREE.Vector2(1, 1) },
  uTexNextRes: { value: new THREE.Vector2(1, 1) },
  uProgress: { value: 0.0 },
  uResolution: { value: new THREE.Vector2() },
  uWaveFreq: { value: rippleConfig.waveFreq },
  uWavePow: { value: rippleConfig.wavePow },
  uWaveWidth: { value: rippleConfig.waveWidth },
  uFalloff: { value: rippleConfig.falloff },
  uBoostStrength: { value: rippleConfig.boostStrength },
  uCrossfadeWidth: { value: rippleConfig.crossfadeWidth },
  uMobile: { value: 0.0 },
}

function splitTitle(container) {
  if (!container) return null
  const heading = container.querySelector(".slide-title h1")
  if (!heading) return null

  return new SplitType(heading, {
    types: "words, chars",
    wordClass: "word",
    charClass: "char",
  })
}

function splitDescription(container) {
  if (!container) return []
  const paragraphs = container.querySelectorAll(".slide-description p")
  const allLines = []

  paragraphs.forEach((p) => {
    const split = new SplitType(p, {
      types: "lines",
      lineClass: "line",
    })
    allLines.push(...split.lines)
  })

  return allLines
}

function animateTextOut(container) {
  const titleSplit = splitTitle(container)
  const lines = splitDescription(container)
  const tl = gsap.timeline()

  if (titleSplit) {
    tl.to(titleSplit.chars, {
      y: "-100%",
      duration: 0.6,
      stagger: 0.02,
      ease: "power2.inOut",
    })
  }

  tl.to(
    lines,
    { y: "-100%", duration: 0.6, stagger: 0.02, ease: "power2.inOut" },
    0.1,
  )

  return tl
}

function animateTextIn(container) {
  const titleSplit = splitTitle(container)
  const lines = splitDescription(container)
  const chars = titleSplit ? titleSplit.chars : []

  gsap.set([chars, lines], { y: "100%" })
  gsap.set(container, { opacity: 1 })

  return gsap
    .timeline()
    .to(chars, {
      y: "0%",
      duration: 0.5,
      stagger: 0.02,
      ease: "power2.inOut",
    })
    .to(
      lines,
      { y: "0%", duration: 0.5, stagger: 0.05, ease: "power2.out" },
      0.1,
    )
}

function getMaxCornerDist() {
  const ratio = window.innerHeight / window.innerWidth
  const cx = 0.5
  const cy = 0.5 * ratio
  return Math.sqrt(cx * cx + cy * cy)
}

function handleResize() {
  if (!sliderRef.value || !renderer) return
  const width = sliderRef.value.clientWidth
  const height = sliderRef.value.clientHeight
  renderer.setSize(width, height)
  uniforms.uResolution.value.set(width, height)
  uniforms.uMobile.value = window.innerWidth <= 1000 ? 1.0 : 0.0
  rippleConfig.endValue = getMaxCornerDist() + rippleConfig.waveWidth
  rippleConfig.duration = window.innerWidth <= 1000 ? 1.5 : 3.0
}

const transition = () => {
  if (isTransitioning.value || textures.length < 2) return
  isTransitioning.value = true

  if (rippleTween) {
    rippleTween.kill()
    uniforms.uProgress.value = 0.0
    rippleTween = null
  }

  const nextIndex = (currentIndex.value + 1) % slides.length
  const currentSlideEl = slideContentRef.value

  const exitTimeline = animateTextOut(currentSlideEl)

  const currentTex = textures[currentIndex.value]
  const nextTex = textures[nextIndex]

  material.uniforms.uTexCurrent.value = currentTex
  material.uniforms.uTexNext.value = nextTex
  material.uniforms.uTexCurrentRes.value.set(currentTex.image.width, currentTex.image.height)
  material.uniforms.uTexNextRes.value.set(nextTex.image.width, nextTex.image.height)
  material.uniforms.uProgress.value = 0.0

  let clickUnlocked = false

  rippleTween = gsap.to(uniforms.uProgress, {
    value: rippleConfig.endValue,
    duration: rippleConfig.duration,
    ease: rippleConfig.ease,
    delay: 0.3,
    onUpdate() {
      if (!clickUnlocked && uniforms.uProgress.value > 0.7) {
        clickUnlocked = true
        currentIndex.value = nextIndex
        isTransitioning.value = false
      }
    },
  })

  exitTimeline.then(async () => {
    currentIndex.value = nextIndex
    await nextTick()
    if (slideContentRef.value) {
      animateTextIn(slideContentRef.value)
    }
  })
}

function render() {
  if (renderer && scene && camera) {
    renderer.render(scene, camera)
  }
  animationFrameId = requestAnimationFrame(render)
}

onMounted(async () => {
  if (!sliderRef.value) return

  scene = new THREE.Scene()
  camera = new THREE.OrthographicCamera(-0.5, 0.5, 0.5, -0.5, 0.01, 10)
  camera.position.z = 1

  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  renderer.setClearColor(0x000000, 0)
  sliderRef.value.prepend(renderer.domElement)

  const textureLoader = new THREE.TextureLoader()
  textures = await Promise.all(
    slides.map(
      (slide) =>
        new Promise((resolve) =>
          textureLoader.load(slide.image, (tex) => {
            tex.minFilter = THREE.LinearFilter
            tex.magFilter = THREE.LinearFilter
            tex.wrapS = THREE.ClampToEdgeWrapping
            tex.wrapT = THREE.ClampToEdgeWrapping
            resolve(tex)
          })
        )
    )
  )

  if (textures[0]) {
    uniforms.uTexCurrentRes.value.set(textures[0].image.width, textures[0].image.height)
  }
  if (textures[1]) {
    uniforms.uTexNextRes.value.set(textures[1].image.width, textures[1].image.height)
  }

  material = new THREE.ShaderMaterial({
    vertexShader,
    fragmentShader,
    uniforms: {
      ...uniforms,
      uTexCurrent: { value: textures[0] },
      uTexNext: { value: textures[1] },
    },
    transparent: true,
  })

  plane = new THREE.Mesh(new THREE.PlaneGeometry(1, 1), material)
  scene.add(plane)

  window.addEventListener("resize", handleResize)
  handleResize()

  await nextTick()
  if (slideContentRef.value) {
    const initialTitle = splitTitle(slideContentRef.value)
    const initialLines = splitDescription(slideContentRef.value)

    if (initialTitle) {
      gsap.fromTo(
        initialTitle.chars,
        { y: "100%" },
        { y: "0%", duration: 0.8, stagger: 0.025, ease: "power2.out" }
      )
    }

    if (initialLines.length > 0) {
      gsap.fromTo(
        initialLines,
        { y: "100%" },
        { y: "0%", duration: 0.8, stagger: 0.025, ease: "power2.out", delay: 0.2 }
      )
    }
  }

  render()
})

onUnmounted(() => {
  window.removeEventListener("resize", handleResize)
  if (animationFrameId) cancelAnimationFrame(animationFrameId)
  if (rippleTween) rippleTween.kill()
  
  if (plane) {
    scene.remove(plane)
    plane.geometry.dispose()
  }
  if (material) material.dispose()
  textures.forEach((tex) => tex.dispose())
  if (renderer) renderer.dispose()
})
</script>

<template>
  <section
    id="webgl-slider"
    ref="sliderRef"
    data-cursor="view"
    data-cursor-text="NEXT"
    class="relative w-full h-screen bg-white overflow-hidden select-none cursor-pointer"
    @click="transition"
  >
    <!-- Overlay Content -->
    <div
      ref="slideContentRef"
      class="slide-content absolute inset-0 z-10 pointer-events-none mix-blend-difference"
    >
      <!-- Slide Title -->
      <div class="slide-title absolute top-1/2 left-8 md:left-12 -translate-y-1/2 text-white max-w-[80vw]">
        <h1 class="text-4xl sm:text-6xl md:text-8xl font-medium tracking-tight leading-[1.1] font-[poppins]">
          {{ slides[currentIndex].title }}
        </h1>
      </div>

      <!-- Slide Description -->
      <div class="slide-description absolute top-1/2 right-8 md:right-8 -translate-y-1/2 w-[75%] md:w-[22%] min-w-[240px] text-white flex flex-col gap-4">
        <p class="text-sm md:text-base font-medium leading-relaxed font-[poppins]">
          {{ slides[currentIndex].description }}
        </p>
      </div>

      <!-- Slide Counter Indicator -->
      <div class="absolute bottom-8 left-8 md:left-12 text-white text-xs md:text-sm font-medium tracking-widest uppercase font-[poppins]">
        0{{ currentIndex + 1 }} / 0{{ slides.length }}
      </div>
    </div>
  </section>
</template>

<style scoped>
.slide-content {
  user-select: none;
}

:deep(.char),
:deep(.line) {
  display: inline-block;
  will-change: transform;
  position: relative;
}

@media (max-width: 1000px) {
  .slide-title {
    top: 40% !important;
    left: 50% !important;
    transform: translate(-50%, -50%) !important;
    text-align: center;
  }

  .slide-description {
    width: 80% !important;
    text-align: center;
    top: unset !important;
    bottom: 12% !important;
    left: 50% !important;
    transform: translateX(-50%) !important;
  }
}
</style>
