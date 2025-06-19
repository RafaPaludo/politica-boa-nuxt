<template>
  <div class="layout-wrapper"> <!-- Adicione esta div wrapper -->
    <AppHeader />
    <UMain>
      <Transition name="page" mode="out-in">
        <div :key="$route.fullPath"> <!-- Key única baseada na rota -->
          <slot />
        </div>
      </Transition>
    </UMain>
    <AppFooter />
  </div>
</template>

<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.4s ease;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
