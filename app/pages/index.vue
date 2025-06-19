<script setup lang="ts">
import image1 from '~/assets/img/planejamento_estrategico_home.jpg'
import image2 from '~/assets/img/mentoria_individual_home.jpg'
import image3 from '~/assets/img/conteudos_politicos_e_organizacionais_home.jpg'
import image4 from '~/assets/img/acessoria_marketing_digital_home.jpg'
import image5 from '~/assets/img/solucoes_tecnologicas_home.jpg'
import profPaludo from '~/assets/img/prof_paludo.png'

const { data: page } = await useAsyncData('index', () => queryCollection('index').first())

const sectionImages = [
  {
    src: image1,
    alt: 'Uma homem fazendo planejamento estratégico em frente a um quadro branco com anotações.'
  },
  {
    src: image2,
    alt: 'Homem fazendo uma mentoria individual pelo computador.'
  },
  {
    src: image3,
    alt: 'Mão segurando um leitor de textos digitais.'
  },
  {
    src: image4,
    alt: 'Tela de um computador com ferramentas de marketing.'
  },
  {
    src: image5,
    alt: 'Engenheira de software codificando em sua mesa com vários monitores'
  }
]

const cardUI = {
  root: 'my-1 ml-1',
  footer: 'text-center'
}

const heroUI = {
  container: 'py-16 sm:py-24 lg:pt-4 gap-0 sm:gap-y-0 relative'
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
    <UPageHero
      :title="page.hero.title"
      :description="page.hero.description"
      orientation="horizontal"
      :ui="heroUI"
    >
      <img
        :src="profPaludo"
        alt="Foto do Professor Paludo"
        class="rounded-lg "
      >
      <LazyStarsBg />
      <div class="w-full col-span-2 flex flex-col gap-2 lg:absolute lg:bottom-[3vh]">
        <SubscribeForm />
        <a href="#feature-0">
          <UIcon
            name="i-ri-arrow-down-wide-fill"
            class="mt-4 size-12 mx-auto col-span-2 animate-bounce hidden lg:block"
          />
        </a>
      </div>
    </UPageHero>

    <UPageSection
      v-for="(section, index) in page.sections"
      :id="`feature-${index}`"
      :key="index"
      :title="section.title"
      :description="section.description"
      :orientation="section.orientation"
      :reverse="section.reverse"
      :features="section.features"
      :ui="{
        description: 'font-bold'
      }"
    >
      <img
        :src="sectionImages[index]?.src"
        :alt="sectionImages[index]?.alt"
        class="rounded-lg m-auto"
      >
    </UPageSection>

    <USeparator />

    <!-- Área do líder -->
    <UPageSection
      :title="page.leader.title"
      :description="page.leader.description"
      :ui="{
        description: 'font-bold',
        root: 'bg-primary-50 dark:bg-primary-950'
      }"
      :features="page.leader.features"
      :links="page.leader.links"
      orientation="horizontal"
    >
      <img
        :src="page.leader.image"
        width="352"
        height="647"
        alt="Foto do professor Paludo"
        class="w-full rounded-lg"
      >
    </UPageSection>

    <!-- Área dos membros -->
    <UPageSection
      :title="page.members.title"
      :description="page.members.description"
      :ui="{
        container: 'lg:py-32 lg:px-0',
        root: 'bg-primary-50 dark:bg-primary-950'
      }"
    >
      <UCarousel
        v-slot="{ item }"
        loop
        arrows
        dots
        class-names
        wheel-gestures
        :items="page.members.items"
        :ui="{
          item: 'xl:basis-1/3 transition-opacity [&:not(.is-snapped)]:opacity-20',
          dots: 'xl:hidden',
          arrows: 'hidden xl:block'
        }"
      >
        <UCard
          variant="outline"
          :ui="cardUI"
        >
          <img
            :src="item.src"
            width="300"
            height="300"
            class="rounded-lg m-auto"
            :alt="`Foto de ${item.title}`"
          >
          <template #footer>
            <h4 class="font-bold text-2xl">
              {{ item.title }}
            </h4>

            <h5 class="mb-4">
              {{ item.work }}
            </h5>

            <h5 class="mb-4 min-h-[120px] text-sm">
              {{ item.description }}
            </h5>
            <div class="mt-4">
              <UButton
                v-for="link in item.links"
                :key="link.label"
                :icon="link.icon"
                :to="link.to"
                color="secondary"
                variant="outline"
                size="xl"
                class="mx-2"
              />
            </div>
          </template>
        </UCard>
      </UCarousel>
    </UPageSection>

    <!-- <UPageSection
      :title="page.features.title"
      :description="page.features.description"
    >
      <UPageGrid>
        <UPageCard
          v-for="(item, index) in page.features.items"
          :key="index"
          v-bind="item"
          spotlight
        />
      </UPageGrid>
    </UPageSection> -->

    <!--
      <UPageSection
        id="testimonials"
        :headline="page.testimonials.headline"
        :title="page.testimonials.title"
        :description="page.testimonials.description"
      >
        <UPageColumns class="xl:columns-4">
          <UPageCard
            v-for="(testimonial, index) in page.testimonials.items"
            :key="index"
            variant="subtle"
            :description="testimonial.quote"
            :ui="{ description: 'before:content-[open-quote] after:content-[close-quote]' }"
          >
            <template #footer>
              <UUser
                v-bind="testimonial.user"
                size="lg"
              />
            </template>
          </UPageCard>
        </UPageColumns>
      </UPageSection>
    -->

    <USeparator />

    <UPageCTA
      v-bind="page.cta"
      variant="naked"
      class="overflow-hidden"
      orientation="horizontal"
      :links="page.cta.links"
      :ui="{
        container: 'lg:items-start'
      }"
    >
      <div class="absolute rounded-full dark:bg-(--ui-primary) blur-[250px] size-40 sm:size-50 transform -translate-x-1/2 left-1/2 -translate-y-80" />

      <LazyStarsBg />

      <SubscribeFormCta />
    </UPageCTA>
  </div>
</template>
