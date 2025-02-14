<template>
  <UFooter>
    <template #left>
      <p class="text-gray-500 dark:text-gray-400 text-sm">
        Copyright © {{ new Date().getFullYear() }}. Todos os direitos reservados.
      </p>
    </template>

    <template #right>
      <UColorModeButton size="sm" />

      <UButton
        to="https://www.instagram.com/institutopoliticaboa/"
        target="_blank"
        icon="i-simple-icons-instagram"
        aria-label="Instagram"
        color="gray"
        variant="ghost"
      />
    </template>
  </UFooter>
</template>
