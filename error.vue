<template>
  <div class="error-page">
    <div class="error-page__container">
      <header>
        <h1 class="error-page__title"> Página não encontrada</h1>
        <h2 class="error-page__subtitle">Você caminhou até lugares muito, muito distantes.</h2>
      </header>

      <NuxtLink class="error-page__link" to="/">Voltar para casa</NuxtLink>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.error-page {
  background-image: url('~/assets/images/error-wallpaper.jpeg');
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;

  &__container {
    padding: 24px;
    background-color: $primary;
    border-radius: 24px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 32px;
  }

  &__title {
    color: white;
    font-size: 3rem;
    text-align: center;
  }

  &__subtitle {
    color: white;
    font-size: 1rem;
    text-align: center;
  }

  &__link {
    color: black;
  }
}
</style>
