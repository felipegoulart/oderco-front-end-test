<script lang="ts" setup>
import type { CardType } from '~/types/people';

const {
  height,
  mass,
  name,
  url
} = defineProps<CardType>()

const id = ref<string>()

id.value = url.replace(/\D/g, '')

</script>

<template>
  <div class="app-card">
    <header>
      <h3 class="app-card__title">
        {{ name }}
      </h3>
    </header>

    <div class="app-card__text-container">
      <p class="app-card__text">Altura: {{ height }}cm</p>
      <p class="app-card__text">Peso: {{ mass }}kg</p>
    </div>

    <footer>
      <AppLink :path="`/character/${id}`" text="Ver todos os detalhes" />
    </footer>
  </div>
</template>

<style lang="scss" scoped>
.app-card {
  border: solid 1px #fff;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  gap: 8px;
  height: 182px;
  padding: 40px 64px;
  width: 328px;

  &__title {
    font-size: 1.5rem;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  &__text-container {
    flex: 1;
  }

  &__text {
    color: $secondary;
  }
}
</style>
