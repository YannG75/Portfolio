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
    image: 'pastitalia.png',
    description: `Pastitalia is a website for an Italian restaurant based in
        New York. It features the menu, location, and contact information.`,
    accurateDescription: `It's a website for an italian restaurant
        based in New York. It is entirely made with HTML, CSS, and JavaScript
        and the content is managed with the wordpress CMS.`,
    technos: ['HTML', 'CSS', 'JavaScript', 'Wordpress'],
    link: 'https://pastitaliaus.com'
  },
  {
    name: 'Busterwood',
    image: 'busterwood.png',
    description: `A creative website that presents what Busterwood
        is all about and what they do.`,
    accurateDescription: `A creative website that presents Busterwood as a creative agency that specializes
        in branding, web design, and digital marketing. The website is built with
        HTML, CSS, and JavaScript.`,
    technos: ['HTML', 'CSS', 'JavaScript'],
    link: 'https://busterwood.com'
  },
  {
    name: 'Weather App',
    image: 'weather.png',
    description: `A simple weather application that provides
        current weather information based on provided location name.`,
    technos: ['HTML', 'CSS', 'JavaScript', 'React', 'OpenWeatherMap API'],
    link: 'https://weather-app-react-night.vercel.app/',
    github: 'https://github.com/YannG75/weather-app-react'
  },
  {
    name: 'todoApp',
    image: 'todo.png',
    description: `A simple todo application that allows you to
        create, edit, delete tasks and mark them as completed.`,
    technos: ['HTML', 'CSS', 'JavaScript', 'React'],
    link: 'https://todo-app-react-violet.vercel.app/',
    github: 'https://github.com/YannG75/TodoApp-React'
  }
])
</script>

<template>
  <section id="works" class="w-full min-h-screen flex flex-col items-center justify-center">
    <div class="w-full flex flex-col items-start justify-center">
      <h2 class="text-xl ml-4 sm:ml-[6%] font-[Sora] font-thin text-gray-400 mb-5">My works</h2>
      <article class="flex flex-col items-start w-full">
        <div
            v-for="work in works"
            ref="target" :key="work.name"
            class="flex h-[100px] cursor-pointer transition-[height] duration-250 relative justify-between items-center w-full mb-5 py-2 group"
            :class="clicked == work.name ? '!h-[400px] sm:!h-[450px] md:!h-[500px] flex-col justify-center gap-10': ''" @click="changeWork( work.name)"
            @mouseover="emit('update:hided', true)" @mouseleave="emit('update:hided', false)">
          <div
              :style="getBackgroundStyle(work.name, work.image)"
              :class="clicked == work.name ? 'h-full' : 'bg-primary'"
              class="opacity-90 absolute w-full h-0 group-hover:h-full group-focus:h-full transition-[height] duration-300 z-0 ease-in-out"
          />
          <h3
              :class="clicked == work.name ? '!ml-0' : '' "
              class="font-[Sora] z-10 font-black text-3xl sm:text-4xl md:text-6xl ml-4 sm:ml-[6%] whitespace-pre-line">
            {{ work.name }}
          </h3>
          <p
              :class="clicked == work.name ? 'visible !mr-0 !text-center' : '' "
              class="text-right z-10 mr-4 sm:mr-[10%] md:mr-[15%] text-sm sm:text-base text-[#E5E5E5] whitespace-pre-line invisible group-hover:visible group-focus:visible">
            {{ clicked == work.name && work.accurateDescription ? work.accurateDescription : work.description }}
          </p>
          <div v-if="clicked == work.name" class="flex gap-4 sm:gap-8 md:gap-5 z-10 justify-center items-center">
              <a class="bg-[#E5E5E5] font-[sora] text-[#545454] text-sm sm:text-base px-4 sm:px-5 py-2 rounded-md shadow-xl " :href="work.link" target="_blank">check it out</a>
              <a v-if="work.github" class="bg-[#E5E5E5] text-[#545454] px-4 sm:px-5 py-1 rounded-md shadow-xl" :href="work.github" target="_blank">
                <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 sm:w-8 sm:h-8" viewBox="0 0 24 24">
                  <!-- Icon from Material Design Icons by Pictogrammers - https://github.com/Templarian/MaterialDesign/blob/master/LICENSE -->
                  <path
                      fill="currentColor"
                      d="M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5c.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34c-.46-1.16-1.11-1.47-1.11-1.47c-.91-.62.07-.6.07-.6c1 .07 1.53 1.03 1.53 1.03c.87 1.52 2.34 1.07 2.91.83c.09-.65.35-1.09.63-1.34c-2.22-.25-4.55-1.11-4.55-4.92c0-1.11.38-2 1.03-2.71c-.1-.25-.45-1.29.1-2.64c0 0 .84-.27 2.75 1.02c.79-.22 1.65-.33 2.5-.33s1.71.11 2.5.33c1.91-1.29 2.75-1.02 2.75-1.02c.55 1.35.2 2.39.1 2.64c.65.71 1.03 1.6 1.03 2.71c0 3.82-2.34 4.66-4.57 4.91c.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2"/>
                </svg>
              </a>
          </div>

        </div>
      </article>
    </div>
  </section>
</template>