<script lang="ts" setup>
interface SearchParamsType {
  resource: string,
  search: string
}

const route = useRoute()
const query = ref<SearchParamsType>({ resource: '', search: '' })

const resources = [
  { label: 'Filmes', value: 'films' },
  { label: 'Personagens', value: 'people' },
  { label: 'Planetas', value: 'planets' },
  { label: 'Espécies', value: 'species' },
  { label: 'Naves espaciais', value: 'starships' },
  { label: 'Veículos', value: 'vehicles' }
]

const data = ref({})
const list = ref([])

async function fetchData({ resource, search = '' }: SearchParamsType) {
  const query = `${resource}/?search=${search}`

  return $fetch(`https://swapi.dev/api/${query}`)
}

function handleQueryOnInit() {
  const { resource = 'films', search = '' } = route.query

  query.value = { resource, search }
}

onBeforeMount(async () => {
  handleQueryOnInit()

  data.value = await fetchData(query.value)

  list.value = data.value.results
})

</script>

<template>
  <div class="index-page">
    <header class="index-page__header">
      <h1 class="index-page__title">Todos os dados de Star Wars que você sempre quis:</h1>
      <h2 class="index-page__subtitle">Planetas, naves espaciais, veículos, pessoas, filmes e espécies</h2>
    </header>

    <SearchBar>
      <template #prepend>
        <AppSelect :options="resources" />
      </template>
    </SearchBar>

    <pre>{{ data.results }}</pre>

    <AppList :items="list" />
  </div>
</template>

<style lang="scss" scoped>
.index-page {
  display: flex;
  flex-direction: column;
  gap: 72px;

  &__header {
    display: flex;
    flex-direction: column;
    gap: 16px;
    text-align: center;
  }

  &__title {
    font-size: 2rem;
    color: white;
  }

  &__subtitle {
    font-size: 1.5rem;
    color: $secondary;
  }
}
</style>
