<script lang="ts" setup>
defineProps<{
  label: string
  value: string | number | undefined
}>()
</script>

<template>
  <p class="app-info__label">
    {{ label }}
    <span class="app-info__value">
      {{ value ?? 'Não informado' }}
    </span>
  </p>
</template>

<style lang="scss" scoped>
.app-info {
  &__label {
    color: $primary;
    font-size: 1.5rem;
  }

  &__value {
    color: white;
    font-size: 2rem;
  }
}
</style>
