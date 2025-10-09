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
      src: 'https://optin.entregaemails.com.br/accounts/200659/forms/3',
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
    const container = document.getElementById('opt_3');
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
        const container = document.getElementById('opt_3');

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
        orientation="horizontal"
      >
        <img
          :src="lp.image.src"
          alt="App screenshot"
          class="rounded-lg shadow-2xl ring ring-default"
        />

        <template #title>
          <h2 class="text-7xl font-bold text-(--ui-primary)">
            {{ lp.title }}
          </h2>
        </template>

        <template #description>
          <h3 class="text-xl mb-3 font-bold">
            Não perca a próxima Live, dia 12/10 às 18h!
          </h3>

          <p class="mb-3">
            🎉 Tem coisa boa vindo aí... mas é surpresa! 👀
          </p>

          <p class="mb-3">
            No próximo domingo vai rolar uma live daquelas — cheia de novidades, conteúdos incríveis e umas surpresinhas que você vai amar.
            Mas olha... é tudo segredo por enquanto! 🤫
          </p>
          <p class="mb-3">
            Quer saber quando a live vai começar?
            Se inscreve no formulário aí embaixo e entra no nosso grupo do WhatsApp.
            A gente vai te avisar por lá.
          </p>

          <p class="mb-3">
            📌 O link da transmissão será enviado apenas para quem se cadastrar no grupo de WhatsApp através do <strong>formulário abaixo</strong>.
            Garanta já sua participação e convide seus colegas.
          </p>

          <p class="text-5xl text-center mt-20">
            <UIcon
              name="i-solar-double-alt-arrow-down-line-duotone"
              class="mt-4 size-12 mx-auto col-span-2 animate-bounce hidden lg:block"
            />
          </p>
        </template>
      </UPageHero>

      <div v-if="formSuccess">
        <h3 class="text-2xl mb-1 text-center mx-auto">Sucesso!</h3>

        <p class="py-5 text-center text-lg">
          Entre no grupo do whatsapp e receba o link para participar da live
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
        <div id="opt_3" class="h-full"></div>
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