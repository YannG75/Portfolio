<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import Navigation from '~/components/Navigation.vue'
import Hero from '~/components/Hero.vue'
import AboutMe from '~/components/AboutMe.vue'
import Works from '~/components/Works.vue'
import Contact from '~/components/Contact.vue'
import FirefliesBackground from '~/components/FirefliesBackground.vue'

type SectionKey = 'hero' | 'about' | 'works' | 'contact'

const mouseCursor = ref<HTMLDivElement | null>(null)
const hovered = ref(false)
const hided = ref(false)

// section active pour la navigation
const activeSection = ref<SectionKey>('hero')

// refs des sections
const heroRef = ref<HTMLElement | null>(null)
const aboutRef = ref<HTMLElement | null>(null)
const worksRef = ref<HTMLElement | null>(null)
const contactRef = ref<HTMLElement | null>(null)

let observer: IntersectionObserver | null = null

function movingEvent(e: MouseEvent) {
  if (mouseCursor.value) {
    mouseCursor.value.style.transform = `translate(${e.pageX - 10}px, ${e.pageY - 7}px)`
  }
}

onMounted(() => {
  const options: IntersectionObserverInit = {
    root: null,
    threshold: 0.4, // ~40 % visible pour être marqué comme actif
  }

  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return

      const el = entry.target as HTMLElement

      if (el === heroRef.value) {
        activeSection.value = 'hero'
      } else if (el === aboutRef.value) {
        activeSection.value = 'about'
      } else if (el === worksRef.value) {
        activeSection.value = 'works'
      } else if (el === contactRef.value) {
        activeSection.value = 'contact'
      }
    })
  }, options)

  const sections = [heroRef.value, aboutRef.value, worksRef.value, contactRef.value]
  sections.forEach((el) => el && observer?.observe(el))
})

onBeforeUnmount(() => {
  if (observer) {
    observer.disconnect()
    observer = null
  }
})
</script>

<template>
  <!-- Lucioles en arrière-plan -->
  <FirefliesBackground />

  <main
    class="relative w-full flex flex-col"
    @mousemove="movingEvent"
  >
    <div
      ref="mouseCursor"
      :class="[hovered ? 'w-8 h-8 sm:w-10 sm:h-10 cursor-hover' : '', hided ? '!w-0 !h-0 ' : '']"
      class="absolute pointer-events-none opacity-90 w-5 h-5 sm:w-7 sm:h-7 z-20 cursor-transition bg-primary duration-150 ease rounded-full border-primary"
    />

    <!-- On passe activeSection à la navigation -->
    <Navigation
      :hided="hided"
      :active-section="activeSection"
      @update:hided="hided = $event"
    />

    <NuxtImg src="/profil.png" alt="Hero" class="hidden md:block absolute top-0 left-0 md:w-[50vw] md:h-[100vh] object-cover z-0" />

    <!-- Hero -->
    <section id="hero" ref="heroRef">

      <Hero
        :hovered="hovered"
        @update:hovered="hovered = $event"
      />
    </section>

    <!-- About -->
    <section id="about" ref="aboutRef">
      <AboutMe
        :hovered="hovered"
        :hided="hided"
        @update:hovered="hovered = $event"
        @update:hided="hided = $event"
      />
    </section>

    <!-- Works -->
    <section id="works" ref="worksRef">
      <Works
        :hovered="hovered"
        :hided="hided"
        @update:hovered="hovered = $event"
        @update:hided="hided = $event"
      />
    </section>

    <!-- Contact -->
    <section id="contact" ref="contactRef">
      <Contact
        :hovered="hovered"
        :hided="hided"
        @update:hovered="hovered = $event"
        @update:hided="hided = $event"
      />
    </section>
  </main>
</template>
