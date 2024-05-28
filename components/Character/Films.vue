<script lang="ts" setup>
import type { FilmsType } from '~/types/films';

const { films } = defineProps<{
  films: string[]
}>()

const route = useRoute()
const router = useRouter()

const filmsRequest = films.map(url => $fetch<FilmsType>(url))
const filmId = ref<number | null>(null)

const { data, pending } = await useLazyAsyncData('films', () => Promise.all(filmsRequest))

watch(data, (value) => {
  // Como não tem filme com id 0 então pode validar por valores falsy.
  // Se possuir valor, então foi pego da url e não deve definir novo valor.
  if (filmId.value) return

  filmId.value = value?.[0]?.episode_id ?? null
}, {
  immediate: true
})

watch(filmId, (value) => {
  router.push({ query: { ...route.query, film: value } })
})

filmId.value = Number(route.query?.film) || null

const film = computed(() => data.value?.find?.(({ episode_id }) => episode_id === filmId.value))

const options = computed(() => {
  return data.value?.map?.(({ title, episode_id }) => {
    return { label: title, value: episode_id }
  }) || []
})
</script>

<template>
  <div v-if="pending">
    Carregando...
  </div>

  <div v-else class="character-films">
    <AppSelect v-model="filmId" :options="options" />

    <AppInfo label="Episódio" :value="film?.episode_id" />
    <AppInfo label="Diretor" :value="film?.director" />
    <AppInfo label="Produtor" :value="film?.producer" />
    <AppInfo label="Data de lançamento" :value="film?.release_date" />
  </div>
</template>

<style lang="scss" scoped>
.character-films {
  display: flex;
  flex-direction: column;
  gap: 32px;
}
</style>
