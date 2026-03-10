<script setup lang="ts">
import {ref} from 'vue'

defineProps({
  hovered: Boolean,
  hided: Boolean
})

const clicked = ref('')

const emit = defineEmits(['update:hovered', 'update:hided'])

interface Work {
  name: string
  image: string
  description: string,
  accurateDescription?: string,
  technos?: string[],
  link?: string,
  github?: string
}

const getBackgroundStyle = (workName: string, workImage: string) => {
  if (clicked.value === workName) {
    return {
      backgroundImage: `linear-gradient(rgba(0,0,0,0.6), rgba(126,15,255,0.6)), url(/assets/images/${workImage})`,
      filter: 'blur(1px)',
      backgroundSize: 'cover',
      backgroundPosition: '50% 3%',
      backgroundRepeat: 'no-repeat'
    }
  }
  return {}
}

const changeWork = (workName: string) => {
  clicked.value = workName === clicked.value ? '' : workName
}

const works = ref<Work[]>([
  {
    name: 'Pastitalia',
    image: 'pastitalia.webp',
    description: `Site vitrine pour un restaurant italien basé à New York. Menu, localisation et contact.`,
    accurateDescription: `Site vitrine pour un restaurant italien basé à New York. Entièrement réalisé en HTML, CSS et JavaScript, avec un contenu géré via WordPress.`,
    technos: ['HTML', 'CSS', 'JavaScript', 'Wordpress'],
    link: 'https://pastitaliaus.com'
  },
  {
    name: 'Busterwood',
    image: 'busterwood.webp',
    description: `Site créatif qui présente l'identité et les activités de Busterwood.`,
    accurateDescription: `Site créatif présentant Busterwood, agence spécialisée en branding, web design et marketing digital. Réalisé en HTML, CSS et JavaScript.`,
    technos: ['HTML', 'CSS', 'JavaScript'],
    link: 'https://busterwood.com'
  },
  {
    name: 'Road To The Main Stage By Firestone',
    image: 'firestone.webp',
    description: `Site officiel de l'événement Road to the Main Stage by Firestone, édition 2024.`,
    accurateDescription: `Road to the Main Stage est un événement organisé par Firestone en partenariat avec Deezer, offrant à des artistes émergents la chance de se produire sur de grandes scènes.\n\nSite réalisé sous WordPress avec du CSS personnalisé pour coller à l'identité de la marque. J'ai travaillé sur deux éditions, en assurant la maintenance, les mises à jour et les personnalisations visuelles.`,
    technos: ['wordpress', 'CSS'],
    link: 'https://roadtothemainstagebyfirestone.withdeezer.com/fr/'
  },
  {
    name: 'Weather App',
    image: 'weather.webp',
    description: `Application météo simple affichant les conditions actuelles selon une ville saisie.`,
    technos: ['HTML', 'CSS', 'JavaScript', 'React', 'OpenWeatherMap API'],
    link: 'https://weather-app-react-night.vercel.app/',
    github: 'https://github.com/YannG75/weather-app-react'
  },
  {
    name: 'todoApp',
    image: 'todo.webp',
    description: `Application de gestion de tâches : création, édition, suppression et marquage comme complété.`,
    technos: ['HTML', 'CSS', 'JavaScript', 'React'],
    link: 'https://todo-app-react-violet.vercel.app/',
    github: 'https://github.com/YannG75/TodoApp-React'
  }
])
</script>

<template>
  <section id="works" class="w-full min-h-screen flex flex-col items-center justify-center">
    <div class="w-full flex flex-col items-start justify-center">
      <h2 class="text-xl ml-4 sm:ml-[6%] font-['Sora_Variable'] font-thin text-gray-400 mb-5">Mes projets</h2>
      <article class="flex flex-col items-start w-full">
        <div
            v-for="work in works"
            ref="target" :key="work.name"
            class="flex h-[150px]  cursor-pointer transition-[height] duration-300 relative justify-between items-center w-full mb-5 py-2 group"
            :class="clicked == work.name ? '!h-[400px] sm:!h-[450px] md:!h-[500px] flex-col justify-center gap-10': ''"
            @click="changeWork( work.name)"
            @mouseover="emit('update:hided', true)" @mouseleave="emit('update:hided', false)">
          <div
              :style="getBackgroundStyle(work.name, work.image)"
              :class="clicked == work.name ? 'h-full' : 'bg-primary'"
              class="opacity-90 absolute w-full h-0 lg:group-hover:h-full transition-[height] duration-300 z-0 ease-in-out"
          />
          <div
              :class="clicked == work.name ? '!ml-0 text-center' : ''"
              class="flex flex-col ml-4 sm:ml-[6%] z-10">
            <h3 class="font-['Sora_Variable'] font-black text-3xl sm:text-4xl md:text-6xl whitespace-pre-line">
              {{ work.name }}
            </h3>
            <span v-if="clicked !== work.name" class="text-xs text-gray-400 mt-1 tracking-wide">↗ Voir plus</span>
          </div>
          <p
              :class="clicked == work.name ? 'visible !mr-0 !text-center max-w-[90%]' : '' "
              class="text-right z-10 mr-4 sm:mr-[10%] md:mr-[15%] text-sm sm:text-base text-[#E5E5E5] whitespace-pre-line invisible lg:group-hover:visible">
            {{ clicked == work.name && work.accurateDescription ? work.accurateDescription : work.description }}
          </p>
          <div v-if="clicked == work.name" class="flex gap-4 sm:gap-8 md:gap-5 z-10 justify-center items-center">
            <a
                class="bg-[#E5E5E5] font-['Sora_Variable'] text-[#545454] text-sm sm:text-base px-4 sm:px-5 py-2 rounded-md shadow-xl "
                :href="work.link" target="_blank">Voir le projet</a>
            <a
                v-if="work.github"
                class="bg-[#E5E5E5] text-[#545454] px-4 sm:px-5 py-1 rounded-md shadow-xl"
                :href="work.github" target="_blank">
              <NuxtImg src="/assets/icons/github.svg" alt="github logo" class="w-5 h-5 sm:w-6 sm:h-6"/>
            </a>
          </div>

        </div>
      </article>
    </div>
  </section>
</template>