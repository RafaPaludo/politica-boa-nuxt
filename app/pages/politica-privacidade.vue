<script setup lang="ts">
const { data: page } = await useAsyncData('politica-privacidade', () => queryCollection('politicaPrivacidade').first())

const sectionUI = {
  container: 'py-8 sm:py-8 lg:py-8 lg:pb-32 gap-8 sm:gap-8'
}

useSeoMeta({
  titleTemplate: '',
  title: page.value?.title,
  ogTitle: page.value?.title,
  description: page.value?.description,
  ogDescription: page.value?.description
})
</script>

<template>
  <div v-if="page">
    <UPageSection
      :title="page.content.title"
      :description="page.content.subtitle"
      :ui="sectionUI"
    >
      <div
        v-for="(feature, index) in page.content.features"
        :key="index"
        class="lg:px-32"
      >
        <h2 class="font-bold text-2xl">{{ feature.title }}</h2>
        <p>{{ feature.description }}</p>
      </div>
    </UPageSection>
  </div>
</template>
