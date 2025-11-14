<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { gsap } from 'gsap'

const firefliesContainer = ref<HTMLDivElement | null>(null)
const numberOfFireflies = 20
const fireflies = ref<HTMLDivElement[]>([])

const createFirefly = (index: number) => {
  const firefly = document.createElement('div')
  firefly.className = 'firefly'

  // Position initiale aléatoire
  const startX = Math.random() * window.innerWidth
  const startY = Math.random() * window.innerHeight

  firefly.style.cssText = `
    position: absolute;
    left: ${startX}px;
    top: ${startY}px;
    width: ${Math.random() * 4 + 2}px;
    height: ${Math.random() * 4 + 2}px;
    background: var(--ui-primary);
    border-radius: 50%;
    box-shadow: 0 0 ${Math.random() * 10 + 5}px var(--ui-primary);
    opacity: ${Math.random() * 0.5 + 0.3};
    pointer-events: none;
  `

  return firefly
}

const animateFirefly = (firefly: HTMLDivElement) => {
  const duration = Math.random() * 10 + 15 // 15-25 secondes
  const targetX = Math.random() * window.innerWidth
  const targetY = Math.random() * window.innerHeight

  gsap.to(firefly, {
    x: targetX - parseFloat(firefly.style.left),
    y: targetY - parseFloat(firefly.style.top),
    duration,
    ease: 'none',
    onComplete: () => {
      // Recommencer l'animation avec une nouvelle destination
      animateFirefly(firefly)
    }
  })

  // Animation de pulsation de l'opacité
  gsap.to(firefly, {
    opacity: Math.random() * 0.5 + 0.3,
    duration: Math.random() * 3 + 2,
    repeat: -1,
    yoyo: true,
    ease: 'sine.inOut'
  })
}

onMounted(() => {
  if (firefliesContainer.value) {
    // Créer et animer les lucioles
    for (let i = 0; i < numberOfFireflies; i++) {
      const firefly = createFirefly(i)
      fireflies.value.push(firefly)
      firefliesContainer.value.appendChild(firefly)

      // Démarrer l'animation après un délai aléatoire
      setTimeout(() => {
        animateFirefly(firefly)
      }, Math.random() * 2000)
    }
  }
})

onBeforeUnmount(() => {
  // Nettoyer les animations GSAP
  fireflies.value.forEach(firefly => {
    gsap.killTweensOf(firefly)
  })
})
</script>

<template>
  <div
    ref="firefliesContainer"
    class="fixed inset-0 z-0 overflow-hidden pointer-events-none"
  />
</template>

<style scoped>
.firefly {
  filter: blur(1px);
}
</style>
