<script setup lang="ts">

defineProps({
  hovered: Boolean
})

const emit = defineEmits(['update:hovered'])

</script>

<template>
  <section class="flex flex-col items-center justify-center h-screen relative bg-gradient-to-b from-transparent from-70% to-black bg-blur-sm">
    <article class="w-full px-4 sm:px-6 md:w-1/2 md:ml-auto">
      <h1 class="text-xl text-center md:text-left sm:text-2xl md:text-3xl font-bold text-gray-400">
        Yann Grillon
      </h1>
      <p class="group w-full font-bold text-3xl text-center sm:text-5xl md:text-6xl lg:text-7xl mt-4 md:text-left " @mouseover="emit('update:hovered', true)" @mouseout="emit('update:hovered', false)">
        I code funny things <br> on internet <br>
        <span class="glow-effect text-gray-100 text-xl sm:text-2xl md:text-4xl">
        (well.. I try)
      </span>
      </p>
    </article>
  </section>
</template>

<style scoped>

</style>