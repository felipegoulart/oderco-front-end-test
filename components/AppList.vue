<script lang="ts" setup>
import type { CardType } from '~/types/people'

withDefaults(defineProps<{
  list: CardType[]
}>(), {
  list: () => []
})
</script>

<template>
  <div class="list-cards">
    <AppCard v-for="(item, index) in list" :key="index" :height="item.height" :mass="item.mass" :name="item.name"
      :url="item.url" />
  </div>
</template>

<style lang="scss" scoped>
.list-cards {
  display: grid;
  grid-gap: 24px;
  grid-template-columns: repeat(auto-fit, minmax(328px, 1fr));
  justify-content: center;
  justify-items: center;
  padding: 0 24px;
}
</style>
