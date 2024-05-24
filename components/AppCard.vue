<script lang="ts" setup>

</script>

<template>
  <div class="app-card">
    <header>
      <h3 class="app-card__title">
        Anakin
      </h3>
    </header>

    <div>
      <p class="app-card__text">Alguma coisa</p>
      <p class="app-card__text">Alguma coisa 2</p>
    </div>

    <footer>
      <NuxtLink to="/" class="app-card__link">Ver todos os detalhes</NuxtLink>
    </footer>
  </div>
</template>

<style lang="scss" scoped>
.app-card {
  border: solid 1px #fff;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  gap: 8px;
  height: 182px;
  padding: 40px 64px;
  width: 328px;

  &__title {
    font-size: 1.5rem;
    line-height: 18px;
  }

  &__text {
    color: $secondary;
  }

  &__link {
    color: $primary;
    text-decoration: underline;
  }
}
</style>
