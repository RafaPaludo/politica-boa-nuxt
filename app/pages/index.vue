<script setup lang="ts">
const { data: page } = await useAsyncData('index', () => queryContent('/').findOne())

const heroUi = {
  wrapper: 'py-24 sm:py-32 md:py-60 relative'
}

useSeoMeta({
  title: page.value.title,
  ogTitle: page.value.title,
  description: page.value.description,
  ogDescription: page.value.description
})
</script>

<template>
  <div>
    <ULandingHero
      :ui="heroUi"
      :description="page.hero.description"
      :links="page.hero.links"
    >
      <template #title>
        <span v-html="page.hero.title" />
      </template>
    </ULandingHero>

    <ULandingSection
      id="faq"
      :title="page.faq.title"
      :description="page.faq.description"
      class="scroll-mt-[var(--header-height)]"
    >
      <ULandingFAQ
        multiple
        :items="page.faq.items"
        :ui="{
          button: {
            label: 'font-semibold',
            trailingIcon: {
              base: 'w-6 h-6'
            }
          }
        }"
        class="max-w-4xl mx-auto"
      />
    </ULandingSection>
  </div>
</template>
