<script lang="ts" setup>
const { data } = await useAsyncData('user', () => $fetch('https://api.github.com/users/alexislopes'))
</script>

<template>
<div>
  <img :src="data.avatar_url" alt="">
  <div>
    <p>{{ data.name }}</p>
  </div>
</div>
</template>

<style scoped></style>
