<script lang="ts" setup>
defineProps<{
  details: {
    height: string | number
    mass: string | number
    hair_color: string
    skin_color: string
    eye_color: string
  }
}>()
</script>

<template>
  <div class="character-details">
    <AppInfo label="Altura" :value="`${details.height}cm`" />
    <AppInfo label="Peso" :value="`${details.mass}kg`" />
    <AppInfo label="Cor do cabelo" :value="details.hair_color" />
    <AppInfo label="Cor de pele" :value="details.skin_color" />
    <AppInfo label="Cor dos olhos" :value="details.eye_color" />
  </div>
</template>

<style lang="scss" scoped>
.character-details {
  display: flex;
  height: 100%;
  flex-direction: column;
  justify-content: space-between;
}
</style>
