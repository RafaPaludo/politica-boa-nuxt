<template>
  <slot />
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
