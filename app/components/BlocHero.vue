<script setup lang="ts">

defineProps({
  hovered: Boolean
})

const emit = defineEmits(['update:hovered'])

</script>

<template>
  <section class="flex flex-col items-center justify-center h-screen relative bg-gradient-to-b from-transparent from-70% to-black bg-blur-sm">
    <article class="w-full px-4 sm:px-6 md:w-[45%] md:ml-auto md:mr-8 lg:mr-16 max-w-2xl">
      <h1 class="text-xl text-center md:text-left sm:text-2xl md:text-3xl font-bold text-gray-400">
        Yann Grillon
      </h1>
      <p
        class="group font-bold text-4xl text-center sm:text-5xl md:text-6xl lg:text-7xl mt-4 md:text-left leading-tight"
        @mouseover="emit('update:hovered', true)"
        @mouseout="emit('update:hovered', false)"
      >
        <span class="">Développeur Frontend Freelance</span>
      </p>
      <p class="text-gray-300 text-base sm:text-lg md:text-xl mt-5 text-center md:text-left max-w-md">
        Je conçois des sites et interfaces web modernes — React, Vue.js, TypeScript.
      </p>
    </article>
  </section>
</template>

<style scoped>

</style>