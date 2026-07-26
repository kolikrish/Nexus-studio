<script setup>
/**
 * VariableProximity.vue
 * Vue 3 port of React Bits VariableProximity component.
 */
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  label: { type: String, required: true },
  fromFontVariationSettings: { type: String, required: true },
  toFontVariationSettings: { type: String, required: true },
  containerRef: { type: Object, default: null }, // Vue ref() pointing to container element
  radius: { type: Number, default: 150 },
  falloff: { type: String, default: 'linear' }, // 'linear' | 'exponential' | 'gaussian'
  className: { type: String, default: '' },
  onClick: { type: Function, default: null },
  style: { type: Object, default: () => ({}) }
})

const rootRef = ref(null)
defineExpose({ rootRef })

const letterRefs = ref([])
function setLetterRef(el, index) {
  if (el) {
    letterRefs.value[index] = el
  }
}

const mousePosition = { x: -9999, y: -9999 }

function getContainerElement() {
  return props.containerRef?.value || rootRef.value
}

function updateMousePosition(x, y) {
  const targetEl = getContainerElement()
  if (targetEl) {
    const rect = targetEl.getBoundingClientRect()
    mousePosition.x = x - rect.left
    mousePosition.y = y - rect.top
  } else {
    mousePosition.x = x
    mousePosition.y = y
  }
}

function handleMouseMove(ev) {
  updateMousePosition(ev.clientX, ev.clientY)
}

function handleTouchMove(ev) {
  if (ev.touches && ev.touches[0]) {
    const touch = ev.touches[0]
    updateMousePosition(touch.clientX, touch.clientY)
  }
}

const parsedSettings = computed(() => {
  const parseSettings = (settingsStr) =>
    new Map(
      settingsStr
        .split(',')
        .map(s => s.trim())
        .filter(Boolean)
        .map(s => {
          const parts = s.split(' ').filter(Boolean)
          const name = parts[0].replace(/['"]/g, '')
          const value = parseFloat(parts[1])
          return [name, value]
        })
    )

  const fromSettings = parseSettings(props.fromFontVariationSettings)
  const toSettings = parseSettings(props.toFontVariationSettings)

  return Array.from(fromSettings.entries()).map(([axis, fromValue]) => ({
    axis,
    fromValue,
    toValue: toSettings.get(axis) ?? fromValue
  }))
})

function calculateDistance(x1, y1, x2, y2) {
  return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
}

function calculateFalloff(distance) {
  const norm = Math.min(Math.max(1 - distance / props.radius, 0), 1)
  switch (props.falloff) {
    case 'exponential':
      return norm ** 2
    case 'gaussian':
      return Math.exp(-((distance / (props.radius / 2)) ** 2) / 2)
    case 'linear':
    default:
      return norm
  }
}

let frameId = null

function animate() {
  const targetEl = getContainerElement()
  if (targetEl) {
    const containerRect = targetEl.getBoundingClientRect()

    letterRefs.value.forEach((letterEl) => {
      if (!letterEl) return

      const rect = letterEl.getBoundingClientRect()
      const letterCenterX = rect.left + rect.width / 2 - containerRect.left
      const letterCenterY = rect.top + rect.height / 2 - containerRect.top

      const distance = calculateDistance(
        mousePosition.x,
        mousePosition.y,
        letterCenterX,
        letterCenterY
      )

      if (distance >= props.radius) {
        letterEl.style.fontVariationSettings = props.fromFontVariationSettings
        const wghtObj = parsedSettings.value.find(s => s.axis === 'wght')
        if (wghtObj) {
          letterEl.style.fontWeight = wghtObj.fromValue
        }
        return
      }

      const falloffValue = calculateFalloff(distance)
      const newSettings = parsedSettings.value
        .map(({ axis, fromValue, toValue }) => {
          const interpolatedValue = fromValue + (toValue - fromValue) * falloffValue
          return `'${axis}' ${interpolatedValue}`
        })
        .join(', ')

      letterEl.style.fontVariationSettings = newSettings

      // Fallback font-weight dynamic calculation
      const wghtObj = parsedSettings.value.find(s => s.axis === 'wght')
      if (wghtObj) {
        const calculatedWeight = wghtObj.fromValue + (wghtObj.toValue - wghtObj.fromValue) * falloffValue
        letterEl.style.fontWeight = Math.round(calculatedWeight)
      }
    })
  }

  frameId = requestAnimationFrame(animate)
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove)
  window.addEventListener('touchmove', handleTouchMove)
  frameId = requestAnimationFrame(animate)
})

onBeforeUnmount(() => {
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('touchmove', handleTouchMove)
  if (frameId) cancelAnimationFrame(frameId)
})

// Support multiline text label with line breaks (\n)
const linesData = computed(() => {
  let idx = 0
  const lines = props.label.split('\n')
  return lines.map((line) => ({
    words: line.split(' ').map((word) => ({
      letters: word.split('').map((letter) => ({ letter, index: idx++ }))
    }))
  }))
})
</script>

<template>
  <span
    ref="rootRef"
    :class="className"
    :style="{
      display: 'inline-block',
      fontFamily: '\'Roboto Flex\', \'Inter\', sans-serif',
      ...style
    }"
    @click="onClick"
  >
    <template v-for="(line, lineIndex) in linesData" :key="lineIndex">
      <br v-if="lineIndex > 0" />
      <template v-for="(word, wordIndex) in line.words" :key="wordIndex">
        <span class="inline-block whitespace-nowrap">
          <span
            v-for="item in word.letters"
            :key="item.index"
            :ref="(el) => setLetterRef(el, item.index)"
            style="display: inline-block; will-change: font-variation-settings, font-weight; transition: font-variation-settings 0.05s ease-out, font-weight 0.05s ease-out;"
            aria-hidden="true"
          >{{ item.letter }}</span>
          <span v-if="wordIndex < line.words.length - 1" class="inline-block">&nbsp;</span>
        </span>
      </template>
    </template>
    <span class="sr-only">{{ label }}</span>
  </span>
</template>