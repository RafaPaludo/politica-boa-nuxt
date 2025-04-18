<script setup lang="ts">
import { siteInfo } from '~/config/siteInfo'
// const route = useRoute()
// const items = computed(() => [{
//   label: 'Docs',
//   to: '/docs',
//   active: route.path.startsWith('/docs')
// }, {
//   label: 'Pricing',
//   to: '/pricing'
// }, {
//   label: 'Blog',
//   to: '/blog'
// }])
</script>

<template>
  <UHeader>
    <template #left>
      <NuxtLink to="/">
        <LogoPro class="w-auto h-10 shrink-0" />
      </NuxtLink>
    </template>

    <UNavigationMenu
      variant="link"
    />

    <template #right>
      <UColorModeButton />
      <UButton
        variant="ghost"
        color="neutral"
        icon="i-ri-instagram-fill"
        :to="siteInfo.socialLinks.instagram"
      />
      <UButton
        variant="ghost"
        color="neutral"
        icon="i-ri-youtube-fill"
        :to="siteInfo.socialLinks.youtube"
      />
      <UButton
        variant="ghost"
        color="neutral"
        icon="i-ri-tiktok-fill"
        :to="siteInfo.socialLinks.tiktok"
      />
    </template>

    <template #body>
      <UNavigationMenu
        orientation="vertical"
        class="-mx-2.5"
      />

      <USeparator class="my-6" />
    </template>
  </UHeader>
</template>
