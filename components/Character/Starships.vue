<script lang="ts" setup>
import type { StarshipsType } from '~/types/starships';

const { starships } = defineProps<{
  starships: string[]
}>()

const route = useRoute()
const router = useRouter()

const starshipsRequest = starships.map(url => $fetch<StarshipsType>(url))
const starshipName = ref<string | null>(null)

const { data, pending } = await useLazyAsyncData('starship', () => Promise.all(starshipsRequest))

watch(data, (value) => {
  // Como não tem nave com nome ['', 0] então pode validar por valores falsy.
  // Se possuir valor, então foi pego da url e não deve definir novo valor.
  if (starshipName.value) return

  starshipName.value = value?.[0]?.name ?? null
}, {
  immediate: true
})

watch(starshipName, (value) => {
  router.push({ query: { ...route.query, starship: value } })
})

starshipName.value = route.query?.starship as string || null

const starship = computed(() => data.value?.find?.(({ name }) => name === starshipName.value))

const options = computed(() => {
  return data.value?.map?.(({ name, }) => {
    return { label: name, value: name }
  }) || []
})
</script>

<template>
  <div v-if="pending">
    Carregando...
  </div>

  <div v-else class="character-starships">
    <AppSelect v-model="starshipName" :options="options" />

    <AppInfo label="Modelo" :value="starship?.model" />
    <AppInfo label="Fabricante" :value="starship?.manufacturer" />
    <AppInfo label="Preço em créditos" :value="starship?.cost_in_credits" />
    <AppInfo label="Classe" :value="starship?.starship_class" />
  </div>
</template>

<style lang="scss" scoped>
.character-starships {
  display: flex;
  flex-direction: column;
  gap: 32px;
}
</style>
