<script lang="ts" setup>
import { computed, ref, toRefs } from 'vue';

interface Social {
  icon: string //iconify-like icon code
  handler: string
  link: string
}

interface Props {
  github: string
  socials: Social[]
  website: string
}

const props = withDefaults(defineProps<Props>(), {
  github: 'alexislopes',
  website: "https://alexislopes.github.io/alexisme/",
  socials: () => [
    {
      icon: 'tabler:brand-x',
      handler: 'alexislxpes',
      link: 'https://twitter.com/alexoslxpes'
    },
    {
      icon: 'mdi:instagram',
      handler: 'alexislopes',
      link: 'https://www.instagram.com/alexislopes/'
    }
  ]
})

const { github, socials, website } = toRefs(props)

const index = ref(0)

const { data } = await useAsyncData('user', () => $fetch('https://api.github.com/users/' + github.value))

const { pause, resume, isActive } = useIntervalFn(() => {
  index.value++
  if (index.value + 1 > socials.value.length) {
    index.value = 0
  }
}, 3000)

const social = computed(() => socials.value[index.value])
</script>

<template>
  <div class="flex hover:shadow-lg w-fit py-2 px-3 rounded-full gap-4 items-center">
    <img class="rounded-full w-[45px] h-[45px]" :src="data.avatar_url" alt="">
    <div class="relative flex flex-col">
      <span class="text-[8px] text-center mb-[2px]">Developed with 💘 by</span>

      <a :href="website" target="_blank" class="flex items-center gap-2">
        <p class="text-lg hover:underline !leading-3">{{ data.name }}</p>
        <Icon name="fa6-solid:angle-right" class="!text-[11px]" />
      </a>
      <a :href="social.link" target="_blank" class="pt-[5px]">
        <div :key="index" class="flex items-center gap-1">
          <Icon :name="social.icon" />
          <p class="text-sm hover:underline">{{ social.handler }}</p>
        </div>
      </a>
    </div>
  </div>
</template>

<style scoped></style>
