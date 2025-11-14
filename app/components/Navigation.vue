<script setup lang="ts">
import { ref } from 'vue'

interface NavigationMenuItem {
  label: string
  description: string
}

const props = defineProps<{
  hided: boolean
  activeSection: 'hero' | 'about' | 'works' | 'contact'
}>()

const emit = defineEmits(['update:hided'])

const items = ref<NavigationMenuItem[]>([
  {
    label: 'About',
    description: 'Learn more about me',
  },
  {
    label: 'Works',
    description: 'See my latest works and experience',
  },
  {
    label: 'Contact',
    description: 'Get in touch with me',
  },
])

const changeCursor = (hided: boolean) => {
  emit('update:hided', hided)
}

const isActive = (label: string) => {
  const key = label.toLowerCase()
  return (
    (key === 'about' && props.activeSection === 'about') ||
    (key === 'works' && props.activeSection === 'works') ||
    (key === 'contact' && props.activeSection === 'contact')
  )
}
</script>

<template>
  <!-- Logo -->
  <div
    class="fixed top-5 left-5 z-10"
    @mouseenter="changeCursor(true)"
    @mouseleave="changeCursor(false)"
  >
    <nuxt-link :to="{hash:'#hero'}" :external="true" class="flex items-center justify-center cursor-none">
      <NuxtImg
        src="/logoPMini.png"
        alt="Logo"
        class="h-12 w-12 cursor-pointer hover:scale-110 hover:drop-shadow-lg hover:drop-shadow-primary transition-drop-shadow duration-150"
      />
    </nuxt-link>
  </div>

  <!-- Navigation -->
  <nav
    class="fixed top-5 right-5 z-10 flex flex-col gap-2"
    @mouseenter="changeCursor(true)"
    @mouseleave="changeCursor(false)"
  >
    <a
      v-for="item in items"
      :key="item.label"
      :href="`#${item.label.toLowerCase()}`"
      class="relative group px-4"
    >
      <!-- Texte -->
      <span
        class="font-medium text-sm transition-colors duration-200"
        :class="[
          isActive(item.label)
            ? 'text-primary'
            : 'text-gray-800 dark:text-white',
          'group-hover:text-primary'
        ]"
      >
        {{ item.label }}
      </span>

      <!-- Barre verticale -->
      <div
        class="absolute left-0 top-1/2 -translate-y-1/2 w-1 bg-primary transition-all duration-200"
        :class="[
          isActive(item.label) ? 'h-full' : 'h-0',
          'group-hover:h-full'
        ]"
      />
    </a>
  </nav>
</template>

<style scoped>
/* Tu peux garder ça vide si tout est en Tailwind */
</style>