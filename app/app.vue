<script setup lang="ts">
import gsap from 'gsap'
import { ref, onMounted, onBeforeUnmount } from 'vue'
import BlocNavigation from '~/components/BlocNavigation.vue'
import BlocHero from '~/components/BlocHero.vue'
import BlocAboutMe from '~/components/BlocAboutMe.vue'
import BlocWorks from '~/components/BlocWorks.vue'
import BlocContact from '~/components/BlocContact.vue'
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

onMounted(() => {
  // xSetter et ySetter sont des fonctions optimisées créées par GSAP
  const xTo = gsap.quickSetter(mouseCursor.value, "x", "px")
  const yTo = gsap.quickSetter(mouseCursor.value, "y", "px")

  window.addEventListener("mousemove", (e) => {
    if (!mouseCursor.value) return
    if (hovered.value) {
      xTo(e.clientX - 20)
      yTo(e.clientY - 20)
    }
    else {
      xTo(e.clientX - 10)
      yTo(e.clientY - 7)
    }
  })

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
  window.removeEventListener("mousemove", () => {})
})
</script>

<template>
  <!-- Lucioles en arrière-plan -->
  <FirefliesBackground />

  <main
    class="relative w-full flex flex-col"
  >
    <div
      ref="mouseCursor"
      :class="[hovered ? 'w-8 h-8 sm:w-10 sm:h-10 cursor-hover' : '', hided ? '!w-0 !h-0 ' : '']"
      class="fixed will-change-transform pointer-events-none opacity-90 w-5 h-5 sm:w-7 sm:h-7 z-20 cursor-transition bg-primary duration-150 ease rounded-full border-primary"
    />

    <!-- On passe activeSection à la navigation -->
    <BlocNavigation
      :hided="hided"
      :active-section="activeSection"
      @update:hided="hided = $event"
    />

    <NuxtImg src="assets/images/profil.png" alt="Hero" class="hidden md:block absolute top-0 left-0 md:w-[50vw] md:h-[100vh] object-cover z-0" />

    <!-- BlocHero -->
    <section id="hero" ref="heroRef">

      <BlocHero
        :hovered="hovered"
        @update:hovered="hovered = $event"
      />
    </section>

    <!-- About -->
    <section id="about" ref="aboutRef">
      <BlocAboutMe
        :hovered="hovered"
        :hided="hided"
        @update:hovered="hovered = $event"
        @update:hided="hided = $event"
      />
    </section>

    <!-- BlocWorks -->
    <section id="works" ref="worksRef">
      <BlocWorks
        :hovered="hovered"
        :hided="hided"
        @update:hovered="hovered = $event"
        @update:hided="hided = $event"
      />
    </section>

    <!-- BlocContact -->
    <section id="contact" ref="contactRef">
      <BlocContact
        :hovered="hovered"
        :hided="hided"
        @update:hovered="hovered = $event"
        @update:hided="hided = $event"
      />
    </section>
  </main>
</template>
