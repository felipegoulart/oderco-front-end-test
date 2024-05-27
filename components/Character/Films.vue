<script lang="ts" setup>
const { films } = defineProps<{
  films: string[]
}>()

const filmsRequest = films.map(filmURL => $fetch(filmURL))

const filmsData = await useAsyncData('films', () => Promise.all(filmsRequest))


</script>

<template>
  <div>
    <pre>{{ filmsData }}</pre>
    Films
  </div>
</template>

<style lang="scss" scoped></style>
