<script lang="ts" setup>
import { Search, X } from 'lucide-vue-next'

const route = useRoute()
const router = useRouter()
const model = defineModel<string>()

// Atribui o valor para a barra de busca ainda no servidor. 
const { search } = route.query

if (search) {
  model.value = search as string
}

// Faz controle de tempo para atualizar a url query a cada 1 segundo
const currentTimeout = ref<NodeJS.Timeout>()
const DEBOUNCE_TIMER = 1000 // 1 seg

function handleDebounce() {
  currentTimeout.value && clearTimeout(currentTimeout.value)

  const query: { search?: string } = {}

  currentTimeout.value = setTimeout(() => {

    if (model.value) {
      query.search = model.value
    }

    router.push({ query })
  }, DEBOUNCE_TIMER)
}

watch(model, () => {
  handleDebounce()
})

function clearSearch() {
  model.value = ''
}
</script>

<template>
  <div class="search-bar">
    <Search color="#DBA90D" :size="18" />

    <slot name="prepend" />

    <input v-model="model" class="search-bar__input" type="search"
      placeholder="Busque por pessoas, planetas, naves espaciais...">


    <button v-if="model?.length" type="button" title="clear" class="search-bar__clear-button" @click="clearSearch">
      <X :size="16" />
    </button>
  </div>
</template>

<style lang="scss" scoped>
.search-bar {
  align-items: center;
  background-color: #202020;
  border: solid 1px #434343;
  border-radius: 8px;
  display: flex;
  gap: 24px;
  padding: 20px 16px;

  &__input {
    background-color: transparent;
    border: none;
    color: #fff;
    flex: 1 1 0%;
    outline: none;

    &::-webkit-search-cancel-button {
      -webkit-appearance: none;
    }
  }

  &__clear-button {
    border: none;
    background-color: transparent;
    color: #9D9D9D;
    cursor: pointer;
    outline: none;
    transition: 300ms ease-in-out;

    &:hover {
      color: white;
    }
  }
}
</style>
