<script setup lang="ts">
const { data: page } = await useAsyncData('envio-email', () => queryCollection('envioEmail').first())

const formSuccess = ref(false)

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
  <UContainer v-if="page" class="h-lvh flex items-center justify-center">
    <LazyStarsBg />

    <div v-if="formSuccess" class="flex align-center flex-col">
      <h2 class="text-4xl font-bold text-center">Cadastro feito com sucesso!</h2>

      <UButton
        block
        size="xl"
        class="py-5 mt-6"
        to="/"
      >
        Voltar para página inicial
      </UButton>
    </div>

    <div v-else>
      <UPageHero
        :title="page.title"
      >
        <FormsLandingPage v-model="formSuccess"  />
      </UPageHero>
    </div>
  </UContainer>
</template>
