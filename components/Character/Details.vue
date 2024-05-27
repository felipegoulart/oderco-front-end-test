<script lang="ts" setup>
defineProps<{
  details: {
    height: string | number
    mass: string | number
    hair_color: string
    skin_color: string
    eye_color: string
  }
}>()
</script>

<template>
  <div class="character-details">
    <p class="character-details__label">Altura <span class="character-details__value">{{ details.height }}cm</span></p>
    <p class="character-details__label">Peso <span class="character-details__value">{{ details.mass }}kg</span></p>
    <p class="character-details__label">Cor do cabelo <span class="character-details__value">{{ details.hair_color
        }}</span></p>
    <p class="character-details__label">Cor de pele <span class="character-details__value">{{ details.skin_color
        }}</span></p>
    <p class="character-details__label">Cor dos olhos <span class="character-details__value">{{ details.eye_color
        }}</span>
    </p>
  </div>
</template>

<style lang="scss" scoped>
.character-details {
  display: flex;
  height: 100%;
  flex-direction: column;
  justify-content: space-between;

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
