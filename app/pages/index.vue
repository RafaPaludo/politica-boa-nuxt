<script setup lang="ts">
import image1 from '~/assets/img/planejamento_estrategico_home.jpg'
import image2 from '~/assets/img/mentoria_individual_home.jpg'
import image3 from '~/assets/img/conteudos_politicos_e_organizacionais_home.jpg'
import image4 from '~/assets/img/acessoria_marketing_digital_home.jpg'
import image5 from '~/assets/img/solucoes_tecnologicas_home.jpg'

const { data: page } = await useAsyncData('index', () => queryCollection('index').first())

const sectionImages = [
  {
    src: image1,
    alt: 'Uma mulher parada em frente a um quadro branco com notas adesivas'
  },
  {
    src: image2,
    alt: 'Um homem e uma mulher sentados à mesa conversando'
  },
  {
    src: image3,
    alt: 'Mão segurando um leitor de textos digitais'
  },
  {
    src: image4,
    alt: 'Uma mulher parada em frente a um quadro branco com notas adesivas'
  },
  {
    src: image5,
    alt: 'Engenheira de software codifica em sua mesa com computadores'
  }
]

/*
const cardUI = {
  root: 'my-1',
  footer: 'text-center'
}
*/

const heroUi = {
  container: 'lg:bg-[url(/img/professor-paludo.png)] bg-none bg-no-repeat bg-top bg-right py-8 sm:py-16 lg:py-32'
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
      :ui="heroUi"
      orientation="horizontal"
    >
      <template #top>
        <div class="absolute rounded-full dark:bg-(--ui-primary) blur-[300px] size-60 sm:size-80 transform -translate-x-1/2 left-1/2 -translate-y-80" />

        <LazyStarsBg />
      </template>

      <div class="lg:hidden">
        <img
          src="/img/professor-paludo.png"
          alt="Professor Paludo"
          class="w-full h-auto max-w-[400px] mx-auto lg:max-w-[600px] lg:mr-0 lg:ml-auto"
        >
      </div>

      <div class="w-full col-span-2 flex flex-col gap-12">
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
    >
      <img
        :src="sectionImages[index]?.src"
        :alt="sectionImages[index]?.alt"
        class="rounded-lg m-auto"
      >
    </UPageSection>

    <!--
        <UPageSection
          :ui="{ container: 'lg:p-0' }"
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
              >
              <template #footer>
                <h4 class="font-bold text-2xl">
                  {{ item.title }}
                </h4>
    
                <h5 class="mb-4">
                  {{ item.work }}
                </h5>
    
                <h5 class="mb-4 min-h-[96px]">
                  {{ item.description }}
                </h5>
                <div class="mt-4 ">
                  <UButton
                    variant="outline"
                    color="secondary"
                    icon="i-ri-instagram-fill"
                    to="https://www.instagram.com/institutopoliticaboa/"
                    size="xl"
                  />
                  <UButton
                    variant="outline"
                    color="secondary"
                    icon="i-ri-youtube-fill"
                    to="https://www.instagram.com/institutopoliticaboa/"
                    size="xl"
                    class="mx-2"
                  />
                  <UButton
                    variant="outline"
                    color="secondary"
                    icon="i-ri-tiktok-fill"
                    to="https://www.instagram.com/institutopoliticaboa/"
                    size="xl"
                  />
                </div>
              </template>
            </UCard>
          </UCarousel>
        </UPageSection>
    -->

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
    >
      <div class="absolute rounded-full dark:bg-(--ui-primary) blur-[250px] size-40 sm:size-50 transform -translate-x-1/2 left-1/2 -translate-y-80" />

      <LazyStarsBg />
    </UPageCTA>
  </div>
</template>
