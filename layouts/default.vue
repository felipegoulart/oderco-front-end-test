<template>
  <div class="app-container">
    <header class="header">
      <NuxtLink to="/">
        <NuxtImg src="/logo.png" width="148px" />
      </NuxtLink>
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
  flex: 1;
}
</style>
