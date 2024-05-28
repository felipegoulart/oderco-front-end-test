<script lang="ts" setup>
import { ChevronDown } from 'lucide-vue-next';

interface OptionsType {
  label: string
  value: string | number
}

type ModelType = string | number | null | undefined

const { options } = defineProps<{
  options: OptionsType[]
}>()

const model = defineModel<ModelType>()
const show = ref<boolean>(false)
const label = computed(() => options.find(({ value }) => value === model.value)?.label)

function toggleOptions() {
  show.value = !show.value
}

function selectOption(value: ModelType) {
  model.value = value
  toggleOptions()
}
</script>

<template>
  <div class="app-select">
    <div class="app-select__input" @click="toggleOptions()">
      <div class="app-select__label">{{ label }}</div>
      <ChevronDown />
    </div>
    <div v-if="show" class="app-select__dropdown">
      <div v-for="(option, index) of options" :key="index" class="app-select__option"
        @click="selectOption(option.value)">
        {{ option.label }}
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.app-select {
  position: relative;

  &__input {
    border-bottom: solid 1px white;
    color: white;
    display: flex;
    justify-content: space-between;
    padding: 8px 16px;
    cursor: pointer;
  }

  &__dropdown {
    background-color: #151515;
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    border: solid 1px $secondary;
  }

  &__option {
    color: $secondary;
    cursor: pointer;
    padding: 12px 24px;

    &:hover {
      color: #202020;
      background-color: $secondary;
    }
  }
}
</style>
