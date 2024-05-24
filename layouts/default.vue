<template>
  <div class="app-container">
    <header class="header">
      <NuxtImg src="/logo.png" width="148px" />
    </header>

    <main class="content">
      <slot />
    </main>
  </div>
</template>

<style lang="scss" scoped>
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.header {
  background-color: #000;
  border-bottom: solid 2px $primary;
  padding: 24px 64px;
}

.content {
  background-color: #000;
  color: #fff;
  display: grid;
  flex: 1;
  grid-template-columns: 1fr min(1090px, 100%) 1fr;
  padding: 80px 5vmin;

  &>* {
    grid-column: 2;
  }
}
</style>
