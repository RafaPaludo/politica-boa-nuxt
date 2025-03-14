<template>
  <UHeader :links="links">
    <template #logo>
      IPB
    </template>

    <template #right>
      <UButton
        label="Sign in"
        color="gray"
        to="/login"
      />
      <UButton
        label="Sign up"
        icon="i-heroicons-arrow-right-20-solid"
        trailing
        color="black"
        to="/signup"
        class="hidden lg:flex"
      />
    </template>

    <template #panel>
      <UNavigationTree
        :links="mapContentNavigation(navigation)"
        default-open
      />
    </template>
  </UHeader>
</template>

<script setup lang="ts">
import type { NavItem } from '@nuxt/content'

const navigation = inject<Ref<NavItem[]>>('navigation', ref([]))

const links = [{
  label: 'Home',
  to: '/'
}, {
  label: 'Docs',
  to: '/docs'
}, {
  label: 'Preços',
  to: '/pricing'
}, {
  label: 'Blog',
  to: '/blog'
}]
</script>
