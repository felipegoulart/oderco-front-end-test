<script lang="ts" setup>
import type { LocationQueryValue } from 'vue-router';
import type { CardType } from '~/types/people'

interface DataType {
  results: CardType[]
  count: number
  next: string | null
  previous: string | null
}

const route = useRoute()

const list = ref<CardType[]>([])
const listCount = ref<number>(0)
const loading = ref<boolean>(false)
const errors = ref()

function setPeopleList(data: CardType[]) {
  list.value = data
    .map(({ name, mass, height, url }: CardType) => (
      { name, mass, height, url }
    ))
}

async function getList(callback: () => Promise<DataType>) {
  try {
    loading.value = true
    const { results, count: total }: DataType = await callback()

    setPeopleList(results)
    listCount.value = total
  } catch (error) {
    console.error(error)

    errors.value = error
  } finally {
    loading.value = false
  }
}

// Search
const search = ref<string>()

async function getListFromSearchQuery(value: string) {
  setPage(1)
  getList(() => $fetch<DataType>(`https://swapi.dev/api/people/?search=${value}`))
}

watch(
  () => route.query.search,
  value => getListFromSearchQuery(value as string)
)

// Page
const page = ref<number>(1)

async function getListFromPageQuery(page: string) {
  getList(() => $fetch<DataType>(`https://swapi.dev/api/people/?search=${search.value}&page=${page}`))
}

function setPage(value: number | LocationQueryValue) {
  page.value = Number(value ?? 1)
}

watch(
  () => route.query.page,
  value => value && getListFromPageQuery(value as string)
)

// Deve ser vazio caso há pesquisa e a lista seja vazia
const isEmpty = computed(() => !loading.value && list.value.length === 0 && !!search.value)

// Executa ao criar componente do lado do servidor

search.value = route.query?.search as string ?? ''
setPage(route.query?.page as LocationQueryValue)

const { data } = await useFetch<DataType>(`https://swapi.dev/api/people/?search=${route.query.search}&page=${page.value}`)

listCount.value = data.value?.count ?? 0
setPeopleList(data.value?.results || [])
</script>

<template>
  <div class="index-page">
    <header class="index-page__header">
      <h1 class="index-page__title">Todos os dados de Star Wars que você sempre quis:</h1>
      <h2 class="index-page__subtitle2">Planetas, naves espaciais, veículos, pessoas, filmes e espécies</h2>
    </header>

    <SearchBar v-model="search" />

    <div v-if="loading">
      <p>Carregando dados...</p>
    </div>

    <div v-else-if="errors">
      <h2 class="index-page__subtitle">Batemos a nave!</h2>
      <h2 class="index-page__subtitle2">Trabalhando para resolver já estamos. Em alguns minutos voltar você deve.</h2>
    </div>

    <div v-else-if="isEmpty" class="index-page__empty">
      <h2 class="index-page__subtitle">Não conseguimos encontrar nenhum personagem com o termo “{{ search }}”!</h2>
      <p class="index-page__subtitle2">Tente novamente com outro termo de pesquisa.</p>
    </div>

    <AppList v-else :list="list" />

    <AppPagination v-if="listCount > 10" v-model="page" :count="listCount" />
  </div>
</template>

<style lang="scss" scoped>
.index-page {
  display: flex;
  flex-direction: column;
  gap: 72px;

  &__header {
    text-align: center;
  }

  &__title {
    font-size: 2rem;
    color: white;
  }

  &__sybtitle {
    font-size: 1.5rem;
    color: white;
  }

  &__subtitle2 {
    font-size: 1.5rem;
    color: $secondary;
    margin-top: 16px;
  }
}
</style>
