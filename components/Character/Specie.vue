<script lang="ts" setup>
import type { SpeciesType } from '~/types/species';

const { specieUrl } = defineProps<{
  specieUrl: string
}>()

const { data, pending } = await useLazyAsyncData('films', () => $fetch<SpeciesType>(specieUrl))
</script>

<template>
  <div v-if="pending">
    Carregando...
  </div>

  <div v-else class="character-details">
    <AppInfo label="Nome" :value="data?.name" />
    <AppInfo label="Classificação" :value="data?.classification" />
    <AppInfo label="Designação" :value="data?.designation" />
    <AppInfo label="Altura média" :value="data?.average_height" />
    <AppInfo label="Idioma" :value="data?.language" />
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
