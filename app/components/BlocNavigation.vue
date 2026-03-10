<script setup lang="ts">
import { ref } from 'vue'

interface NavigationMenuItem {
  label: string
  anchor: string
  description: string
}

const props = defineProps<{
  hided: boolean
  activeSection: 'hero' | 'about' | 'works' | 'contact'
}>()

const emit = defineEmits(['update:hided'])

const items = ref<NavigationMenuItem[]>([
  {
    label: 'À propos',
    anchor: 'about',
    description: 'En savoir plus sur moi',
  },
  {
    label: 'Projets',
    anchor: 'works',
    description: 'Voir mes derniers projets',
  },
  {
    label: 'Contact',
    anchor: 'contact',
    description: 'Me contacter',
  },
])

const changeCursor = (hided: boolean) => {
  emit('update:hided', hided)
}

const isActive = (anchor: string) => {
  return props.activeSection === anchor
}
</script>

<template>
  <!-- Logo -->
  <div
    class="fixed top-4 left-4 sm:top-5 sm:left-5 z-20"
    @mouseenter="changeCursor(true)"
    @mouseleave="changeCursor(false)"
  >
    <a href="#hero" class="flex items-center justify-center cursor-none">
      <NuxtImg
        src="/assets/images/logoPMini.png"
        alt="Logo"
        class="h-10 w-10 sm:h-12 sm:w-12 cursor-pointer hover:scale-110 hover:drop-shadow-lg hover:drop-shadow-primary transition-drop-shadow duration-150"
        format="webp"
        width="48"
        height="48"
      />
    </a>
  </div>

  <!-- BlocNavigation -->
  <nav
    class="fixed top-4 right-4 sm:top-5 sm:right-5 z-20 flex flex-col gap-2"
    @mouseenter="changeCursor(true)"
    @mouseleave="changeCursor(false)"
  >
    <a
      v-for="item in items"
      :key="item.anchor"
      :href="`#${item.anchor}`"
      class="relative group px-3 sm:px-4 min-h-[30px] flex items-center"
      :class="[isActive(item.anchor) ? 'bg-[rgba(0,0,0,0.2)]' : '']"
    >
      <!-- Texte -->
      <span
        class="font-medium text-base sm:text-sm transition-colors duration-200"
        :class="[
          isActive(item.anchor)
            ? 'text-primary'
            : 'text-white',
          'group-hover:text-primary'
        ]"
      >
        {{ item.label }}
      </span>

      <!-- Barre verticale -->
      <div
        class="absolute left-0 top-1/2 -translate-y-1/2 w-1 bg-primary transition-all duration-200"
        :class="[
          isActive(item.anchor) ? 'h-full' : 'h-0',
          'group-hover:h-full'
        ]"
      />
    </a>
  </nav>
</template>

<style scoped>
/* Tu peux garder ça vide si tout est en Tailwind */
</style>