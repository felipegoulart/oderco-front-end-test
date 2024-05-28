<script lang="ts" setup>
import type { VehiclesType } from '~/types/vehicles';

const { vehicles } = defineProps<{
  vehicles: string[]
}>()

const route = useRoute()
const router = useRouter()

const vehiclesRequest = vehicles.map(url => $fetch<VehiclesType>(url))
const vehicleName = ref<string | null>(null)

const { data, pending } = await useLazyAsyncData('vehicle', () => Promise.all(vehiclesRequest))

watch(data, (value) => {
  // Como não tem veículo com nome ['', 0] então pode validar por valores falsy.
  // Se possuir valor, então foi pego da url e não deve definir novo valor.
  if (vehicleName.value) return

  vehicleName.value = value?.[0]?.name ?? null
}, {
  immediate: true
})

watch(vehicleName, (value) => {
  router.push({ query: { ...route.query, vehicle: value } })
})

vehicleName.value = route.query?.vehicle as string || null

const vehicle = computed(() => data.value?.find?.(({ name }) => name === vehicleName.value))

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

  <div v-else class="character-vehicles">
    <AppSelect v-model="vehicleName" :options="options" />

    <AppInfo label="Modelo" :value="vehicle?.model" />
    <AppInfo label="Fabricante" :value="vehicle?.manufacturer" />
    <AppInfo label="Preço em créditos" :value="vehicle?.cost_in_credits" />
    <AppInfo label="Classe" :value="vehicle?.vehicle_class" />
  </div>
</template>

<style lang="scss" scoped>
.character-vehicles {
  display: flex;
  flex-direction: column;
  gap: 32px;
}
</style>
