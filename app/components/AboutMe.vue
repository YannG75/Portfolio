<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

defineProps({
  hovered: Boolean,
  hided: Boolean
})

interface Thing {
  name: string,
  description: string
}

const clicked = ref('')

// Refs pour les lignes du paragraphe About
const aboutLine1 = ref<HTMLParagraphElement | null>(null)
const aboutLine2 = ref<HTMLParagraphElement | null>(null)
const aboutLine3 = ref<HTMLParagraphElement | null>(null)

const changeClicked = (e: string) => {
  if (clicked.value === e) {
    clicked.value = ''
  }
  else {
    clicked.value = e
  }
}

const emit = defineEmits(['update:hovered', 'update:hided'])

const thingsIdo = ref<Thing[]>([
  {
    name: 'Web Development',
    description: 'I use HTML, CSS, JavaScript, PHP, and more to create websites and web applications.'
  },
  {
    name: 'Snapchat Lens',
    description: 'I can create interactive Snapchat Lens filters and effects using Lens Studio.'
  },
  {
    name: `AR
    experiences`,
    description: 'I can create funny AR experiences on mobile devices.'
  },
  {
    name: 'AI',
    description: 'I use AI to help me build faster and find better solutions.'
  },
])

onMounted(() => {
  const lines = [aboutLine1.value, aboutLine2.value, aboutLine3.value]

  lines.forEach((line) => {
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
  <section id="about" class="w-full min-h-screen flex flex-col items-center justify-center bg-gradient-to-b from-black from-1% to-transparent bg-blur-sm">
    <div class="w-[70%] flex flex-col items-start justify-center mb-20">
      <h2 class="text-xl font-thin font-[Sora] text-gray-400 mb-5">ABOUT ME</h2>
      <p ref="aboutLine1" class="text-gradient-scroll font-bold text-5xl leading-snug w-[90%]">
        I'm a web developer who loves to code and make new things.
      </p>
      <p ref="aboutLine2" class="text-gradient-scroll font-bold text-5xl leading-snug w-[90%]">
        Since i'm lazy i try to be effective and efficient in my work and i'm always looking for new things to learn.
      </p>
      <p ref="aboutLine3" class="text-gradient-scroll font-bold text-5xl leading-snug w-[90%]">
        I use AI to help me achieve my goals faster and better.
      </p>
    </div>
    <div class="w-[100%] flex flex-col items-start justify-center">
      <h2 class="text-xl font-[Sora] ml-[15%] font-thin text-gray-400 mb-5">WHAT I DO</h2>
      <article class="flex flex-col items-start w-full">
        <div v-for="thing in thingsIdo" :key="thing.name" class="flex h-[120px] relative justify-between  items-center w-full py-2 group" @click="changeClicked(thing.name)" @mouseover="emit('update:hided', true)" @mouseleave="emit('update:hided', false)" >
          <div :class="clicked == thing.name ? 'h-full' : ''" class="bg-primary opacity-90 absolute w-full h-0 group-hover:h-full transition-[height] duration-300 z-0 ease-in-out" />
          <h3 class="font-[Sora] font-black ml-[15%] text-5xl whitespace-pre-line z-10 max-[1025px]:w-1/3">{{thing.name}}</h3>
          <p :class="clicked == thing.name ? 'visible' : ''" class="text-right text-[#E5E5E5] mr-[15%] max-[1025px]:w-1/4 lg invisible group-hover:visible group-focus:visible z-10">{{thing.description}}</p>
        </div>
      </article>
    </div>
  </section>
</template>

<style scoped>
.text-gradient-scroll {
  background: linear-gradient(to left, #9ca3af 0%, #9ca3af 50%, var(--ui-primary) 50%, var(--ui-primary) 100%) 100% 0;
  background-size: 200% 100%;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}


</style>