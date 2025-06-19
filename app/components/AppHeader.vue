<script setup lang="ts">
import { siteInfo } from '~/config/siteInfo'

const items = computed(() => [
  {
    label: 'Quem somos',
    icon: 'i-lucide-users',
    to: '/professor-paludo',
    children: [
      {
        label: 'Prof Paludo',
        to: '/professor-paludo'
      }
    ]
  },
  {
    label: 'Blog',
    to: '/blog',
    icon: 'i-lucide-book-open'
  }
])
</script>

<template>
  <UHeader>
    <template #left>
      <NuxtLink
        to="/"
        title="Home"
      >
        <LogoPro class="w-auto h-10 shrink-0 h-[50px]" />
      </NuxtLink>
    </template>

    <UNavigationMenu
      variant="link"
      :items="items"
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
        :items="items"
        orientation="vertical"
        class="-mx-2.5"
      />

      <USeparator class="my-6" />
    </template>
  </UHeader>
</template>
