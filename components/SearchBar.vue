<script lang="ts" setup>
import { Search, X } from 'lucide-vue-next'

const search = ref('')
const route = useRoute()
console.log(route.query)

onMounted(() => {
  const searchQuery = route.query.search

  if (searchQuery) {
    search.value = searchQuery
  }
}),

  function clearSearch() {
    search.value = ''
  }

</script>

<template>
  <div class="search-bar">
    <Search color="#DBA90D" size="18px" />

    <slot name="prepend" />

    <input v-model="search" class="search-bar__input" type="search"
      placeholder="Busque por pessoas, planetas, naves espaciais...">


    <button v-if="search.length" type="button" title="clear" class="search-bar__clear-button" @click="clearSearch">
      <X size="16px" />
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
