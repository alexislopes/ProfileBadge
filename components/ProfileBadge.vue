<script lang="ts" setup>
import { ref, computed } from 'vue'

const index = ref(0)
const socials = ref([
  {
    icon: 'tabler:brand-x',
    handler: 'panzeeh',
    link: 'https://twitter.com/panzeeh'
  },
  {
    icon: 'mdi:instagram',
    handler: 'alexislopes',
    link: 'https://www.instagram.com/alexislopes/'
  }
])

const { data } = await useAsyncData('user', () => $fetch('https://api.github.com/users/alexislopes'))

const { pause, resume, isActive } = useIntervalFn(() => {
  index.value++
  if (index.value + 1 > socials.value.length) {
    index.value = 0
  }

  console.log(index.value)
}, 3000)

const social = computed(() => socials.value[index.value])
</script>

<template>
<div class="flex hover:shadow-md w-fit py-2 px-4  rounded-full gap-4 items-center">
  <img class="rounded-full w-[40px] h-[40px]" :src="data.avatar_url" alt="">
  <div class="relative">
    <a href="https://alexislopes.github.io/alexisme/" target="_blank" class="flex items-center gap-2">
      <p class="text-lg hover:underline">{{ data.name }}</p>
      <Icon name="fa6-solid:angle-right" class="!text-[11px]"/>
    </a>
    <a :href="social.link" target="_blank">
      <div :key="index" class="flex items-center gap-1">
        <Icon :name="social.icon" />
        <p class="text-sm hover:underline">{{ social.handler }}</p>
      </div>
    </a>
  </div>
</div>
</template>

<style scoped></style>
