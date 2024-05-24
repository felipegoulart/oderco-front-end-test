<script lang="ts" setup>
const { options } = defineProps({
  options: {
    type: Array,
    default: () => [],
  }
})

</script>

<template>
  <select class="app-select" name="resource">
    <option v-for="(option, index) of options" :key="index" class="app-select__options" :value="option.value">
      {{ option.label }}
    </option>
  </select>
</template>

<style lang="scss" scoped>
.app-select {
  background-color: #202020;
  border-bottom: solid 1px white;
  border: none;
  color: white;
  outline: none;
  padding: 8px 16px;

  &__options {
    color: $secondary;
  }
}
</style>
