<script setup lang="ts">
const { data: page } = await useAsyncData('professor-paludo', () => queryCollection('professorPaludo').first())

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
      :icon="page.hero.icon"
      :title="page.hero.title"
      :description="page.hero.description"
      :features="page.hero.features"
      :links="page.hero.links"
      orientation="horizontal"
      :ui="sectionUI"
    >
      <img
        v-if="page.hero.image"
        :src="page.hero.image"
        alt="Foto do Professor Paludo"
        class="w-full h-auto object-cover rounded-lg shadow-lg"
      >
    </UPageSection>

    <UPageSection
      :title="page.content.title"
      :ui="sectionUI"
    >
      <p
        v-for="(feature, index) in page.content.features"
        :key="index"
        class="lg:px-32"
      >
        {{ feature.description }}
      </p>
    </UPageSection>
  </div>
</template>
