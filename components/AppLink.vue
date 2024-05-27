<script lang="ts" setup>
defineProps<{
  path: string,
  text: string
}>()
</script>

<template>
  <NuxtLink :to="path" class="app-link">{{ text }}</NuxtLink>
</template>

<style lang="scss" scoped>
.app-link {
  color: $primary;
  text-decoration: underline;
}
</style>
