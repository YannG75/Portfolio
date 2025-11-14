<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

defineProps({
  hovered: Boolean,
  hided: Boolean
})

const emit = defineEmits(['update:hovered','update:hided'])

// Refs pour les paragraphes du motto
const mottoLine1 = ref<HTMLParagraphElement | null>(null)
const mottoLine2 = ref<HTMLParagraphElement | null>(null)
const mottoLine3 = ref<HTMLParagraphElement | null>(null)

onMounted(() => {
  const lines = [mottoLine1.value, mottoLine2.value, mottoLine3.value]

  lines.forEach((line, index) => {
    if (line) {
      gsap.fromTo(line,
        {
          backgroundPosition: '100% 0%'
        },
        {
          backgroundPosition: '0% 0%',
          duration: 1.5,
          ease: 'power2.out',
          scrollTrigger: {
            trigger: line,
            start: 'top 80%',
            end: 'top 40%',
            scrub: 1,
            toggleActions: 'play none none reverse'
          }
        }
      )
    }
  })
})

onBeforeUnmount(() => {
  ScrollTrigger.getAll().forEach(trigger => trigger.kill())
})
</script>

<template>
  <section id="contact" class="w-full min-h-screen flex flex-col items-center justify-center">
    <div class="w-[70%] flex flex-col items-center justify-center mb-20">
      <h2 class="text-4xl font-bold text-gray-400 mb-10">My motto</h2>
      <p ref="mottoLine1" class="text-gradient-scroll font-bold text-5xl leading-snug w-[90%] text-center">
        I'm Doing what I know
      </p>
      <p ref="mottoLine2" class="text-gradient-scroll font-bold text-5xl leading-snug w-[90%] text-center">
        and
      </p>
      <p ref="mottoLine3" class="text-gradient-scroll font-bold text-5xl leading-snug w-[90%] text-center">
        I'm learning what I don't know.
      </p>
    </div>
    <h2 class="text-4xl font-bold text-gray-400 mb-10">Feel free to reach out to me</h2>
    <div class="w-[70%] flex items-center justify-center gap-20">

      <article class="flex flex-col items-center justify-center w-fit relative group" @mouseover="emit('update:hided', true)" @mouseleave="emit('update:hided', false)">
        <span class="z-1 text-bold text-gradient-hover">Email</span>
        <a href="mailto:yannalxr@gmail.com" class="z-1 text-gradient-hover">yannalxr@gmail.com</a>
      </article>
      <article class="relative flex flex-col items-center justify-center w-fit group" @mouseover="emit('update:hided', true)" @mouseleave="emit('update:hided', false)">
        <span class="text-bold z-1 text-gradient-hover">Phone</span>
        <a href="tel:+33645693436" class="z-1 text-gradient-hover">+33 6 45 69 34 36</a>
      </article>
    </div>
  </section>
</template>

<style scoped>
.text-gradient-hover {
  background: linear-gradient(to left, #9ca3af 0%, #9ca3af 50%, var(--ui-primary) 50%, var(--ui-primary) 100%);
  background-size: 200% 100%;
  background-position: 100% 0%;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  transition: background-position 0.4s ease;
}

.group:hover .text-gradient-hover {
  background-position: 0% 0%;
}

.text-gradient-scroll {
  background: linear-gradient(to left, #9ca3af 0%, #9ca3af 50%, var(--ui-primary) 50%, var(--ui-primary) 100%);
  background-size: 200% 100%;
  background-position: 100% 0%;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>