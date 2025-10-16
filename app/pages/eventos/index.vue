<script setup lang="ts">
const { data: page } = await useAsyncData('eventos', () => queryCollection('eventos').first())

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
    <UContainer>
      <UPageHeader
        v-bind="page"
        class="py-[50px]"
      />

      <UPageBody>
        <UBlogPost
          :title="page.event.title"
          :description="page.event.description"
          :image="page.event.image"
          orientation="horizontal"
        >
          <template #date>
            <UIcon name="i-lucide-calendar-clock" />
            {{ page.event.date }}
          </template>

          <template #authors>
            <UButton
              color="primary"
              icon="i-lucide-ticket"
              :to="page.event.links[0]?.to"
            >
              Comprar ingressos
            </UButton>

            <UButton
              color="neutral"
              icon="i-lucide-arrow-up-right"
              to="https://fbis2025.com.br/"
            >
              Ver página do evento
            </UButton>

            <UButton
              color="neutral"
              icon="i-lucide-calendar-clock"
              to="/eventos/fbis/programacao"
            >
              Programação
            </UButton>
          </template>
        </UBlogPost>
      </UPageBody>
    </UContainer>
  </div>
</template>
