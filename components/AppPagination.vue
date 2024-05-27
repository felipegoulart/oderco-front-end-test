<script lang="ts" setup>
const { count } = withDefaults(defineProps<{
  count: number
}>(),
  {
    count: 1,
  }
)

const router = useRouter()
const route = useRoute()
const currentPage = defineModel<number>()

const pageCount = computed(() => Math.ceil(count / 10))

function setPage(page: number) {
  currentPage.value = page

  router.push({
    query: {
      ...route.query,
      page
    }
  })
}

function isActiveClass(page: number) {
  return { 'app-pagination__button--active': page === currentPage.value }
}

// Executa ao criar componente do lado do servidor
currentPage.value = Number(route.query?.page ?? 1)
</script>

<template>
  <div class="app-pagination">
    <span v-for="page of pageCount" :key="page" class="app-pagination__button" :class="isActiveClass(page)"
      @click="() => setPage(page)">
      {{ page }}
    </span>
  </div>
</template>

<style lang="scss" scoped>
.app-pagination {
  display: flex;
  justify-content: end;
  width: 100%;

  &__button {
    align-items: center;
    border: solid 1px $primary;
    cursor: pointer;
    display: flex;
    height: 36px;
    justify-content: center;
    transition: 300ms ease-in-out;
    width: 36px;

    &:first-child {
      border-radius: 8px 0 0 8px;
    }

    &:last-child {
      border-radius: 0 8px 8px 0;
    }

    &:hover {
      background-color: $primary
    }

    &--active {
      background-color: $primary
    }
  }

}
</style>
