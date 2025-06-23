<script setup lang="ts">
const route = useRoute()

const { data: lp } = await useAsyncData(route.path, () => queryCollection('lps').path(route.path).first())
if (!lp.value) {
  throw createError({ statusCode: 404, statusMessage: 'Página não encontrada!', fatal: true })
}

const title = lp.value.seo?.title || lp.value.title
const description = lp.value.seo?.description || lp.value.description
const formSuccess = ref(false)

useSeoMeta({
  title,
  ogTitle: title,
  description,
  ogDescription: description
})

definePageMeta({
  layout: 'slot'
})

if (lp.value.image?.src) {
  defineOgImage({
    url: lp.value.image.src
  })
} else {
  defineOgImageComponent('Default', {
    headline: 'Landing Page'
  })
}
</script>

<template>
  <UContainer v-if="lp" class="h-lvh flex items-center justify-center">
    <LazyStarsBg />

    <div v-if="formSuccess">
      <h2 class="text-4xl font-bold text-center">Sucesso!</h2>
      <p class="py-5 text-center">
        Clique no botão abaixo e baixe seu Manual.
      </p>

      <ULink
        to="https://mcusercontent.com/35835573dce9ecd1ed104ac0a/files/dfd01b67-01c5-c559-eed0-0d6479814c4f/MANUAL_DE_PLANEJAMENTO_ESTRAT_Eacute_GICO_DE_MANDATO.pdf"
        target="_blank"
      >
        <UButton
          block
          size="xl"
          class="py-5"
        >
          Baixar PDF
        </UButton>
      </ULink>
    </div>

    <div v-else>
      <UPageHero
        :title="lp.title"
        :description="lp.description"
        orientation="horizontal"
      >
        <FormsLandingPage v-model="formSuccess"/>
      </UPageHero>
    </div>
  </UContainer>
</template>