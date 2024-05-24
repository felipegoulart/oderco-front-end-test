<script lang="ts" setup>
const { list } = defineProps({
  list: {
    type: Array,
    default: () => []
  }
})
</script>

<template>
  {{ list }}
  <AppCard v-for="(item, index) in list" :key="index" item />
</template>

<style lang="scss" scoped></style>
