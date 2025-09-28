<script setup lang="ts">
const route = useRoute()

const { data: lp } = await useAsyncData(route.path, () => queryCollection('lps').path(route.path).first())

if (!lp.value) {
  throw createError({ statusCode: 404, statusMessage: 'Página não encontrada!', fatal: true })
}

const title = lp.value.seo?.title || lp.value.title
const description = lp.value.seo?.description || lp.value.description
const goToWhats = lp.value?.whats || ''
const formSuccess = ref(false)
const iframeLoadedTimes = ref(0)

useSeoMeta({
  title,
  ogTitle: title,
  description,
  ogDescription: description
})

definePageMeta({
  layout: 'slot'
})

useHead({
  script: [
    {
      src: 'https://optin.entregaemails.com.br/accounts/200659/forms/2',
      async: 'true',
      defer: "true",
      type: 'text/javascript'
    }
  ]
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

const colorMode = useColorMode()

colorMode.value = 'light'

function esperarIframeNaDiv(callback: (iframe: HTMLIFrameElement) => void) {
  const intervalo = setInterval(() => {
    const container = document.getElementById('opt_2');
    if (!container) return;

    const iframe = document.querySelector('.iframe-opt-in') as HTMLIFrameElement | null;

    if (iframe) {
      clearInterval(intervalo);
      callback(iframe);
    }
  }, 300); // verifica a cada 300ms
}

onMounted(() => {
  esperarIframeNaDiv((iframe) => {
    iframe.addEventListener('load', () => {
      iframeLoadedTimes.value = iframeLoadedTimes.value + 1

      if (iframeLoadedTimes.value === 2) {
        const container = document.getElementById('opt_2');

        if (container instanceof HTMLElement) {
          container.style.display = "none";
          formSuccess.value = true
        }
      }
    });
  });
})

</script>

<template>
  <UContainer v-if="lp">
    <LazyStarsBg />
    
    <div class="mb-16">
      <UPageHero
        :title="lp.title"
        :ui="{ container: 'flex flex-col lg:grid py-24 sm:py-32 lg:py-16 gap-16 sm:gap-y-24' }"
        :description="lp.description"
        orientation="horizontal"
      >
        <img
          :src="lp.image.src"
          alt="App screenshot"
          class="rounded-lg shadow-2xl ring ring-default"
        />
      </UPageHero>

      <div v-if="formSuccess">
        <h3 class="text-2xl mb-1">Sucesso!</h3>

        <p class="py-5 text-center">
          Entre no grupo do whatsapp e saiba quando a live for começar.
        </p>

        <ULink
          :to="goToWhats"
          target="_blank"
          class="w-full"
        >
          <UButton
            icon="ic:baseline-whatsapp"
            block
            size="xl"
            class="py-5 w-full"
          >
            Entrar no grupo do whatsapp
          </UButton>
        </ULink>
      </div>

      <div class="iframe-container m-auto" v-else>
        <div id="opt_2" class="h-full"></div>
      </div>
    </div>

    <p class="text-(--ui-text-muted) text-sm text-center py-8">
      Copyright © {{ new Date().getFullYear() }}. Todos os direitos reservados.
    </p>
  </UContainer>
</template>

<style>
.iframe-container {
  height: 440px;
}
</style>