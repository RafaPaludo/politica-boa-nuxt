<script setup lang="ts">
const { data: page } = await useAsyncData('midias', () => queryCollection('midias').first())

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

definePageMeta({
  layout: 'slot'
})
</script>

<template>
  <div>
    <div v-if="page">
      <UPageHero
        :ui="{
          container: 'py-3 sm:py-4 lg:py-5'
        }"
      >
        <div>
          <img
            :src="page.hero?.image"
            width="125"
            height="125"
            alt="Foto do Professor Paludo"
            class="mx-auto rounded-full shadow-2xl ring ring-default"
          />

          <div class="text-center my-10">
            <div class="text-2xl sm:text-4xl text-pretty tracking-tight font-bold text-highlighted">{{ page.hero?.title }}</div>
            <div class="sm:text-xl/8 text-muted">{{ page.hero?.description }}</div>
          </div>

          <UPageList divide>
            <UPageCard
              v-for="(link, index) in page.hero.links"
              :key="index"
              :to="link.to"
              :ui="{
                root: 'mb-3 text-center'
              }"
            >
              <template #body>
                <UButton
                  variant="ghost"
                  color="neutral"
                  class="text-lg"
                  :label="link.label"
                  :icon="link.icon"
                  :to="link.to"
                />
              </template>
            </UPageCard>
          </UPageList>
        </div>  
      </UPageHero>
    </div>
  </div>
</template>
