<template>
  <ClientOnly>
    <UForm
      v-if="showForm"
      :state="state"
      :schema="schema"
      class="lg:gap-4 lg:flex flex-col w-full py-4 px-6 rounded-lg shadow-md dark:bg-(--ui-bg)"
      @submit="onSubmit"
    >
      <h2 class="font-bold text-lg">
        Fique por dentro das novidades. Inscreva-se!
      </h2>
  
      <div class="lg:gap-4 lg:flex w-full">
        <UFormField
          label="Nome completo"
          name="FNAME"
          class="lg:h-[84px] flex-2"
        >
          <UInput
            v-model="state.FNAME"
            placeholder="Seu nome completo"
            class="w-full"
            size="xl"
          />
        </UFormField>
  
        <UFormField
          label="Endereço de e-mail"
          name="EMAIL"
          class="lg:h-[84px] flex-2"
        >
          <UInput
            v-model="state.EMAIL"
            type="email"
            placeholder="seu@email.com"
            class="w-full"
            size="xl"
          />
        </UFormField>
  
        <UFormField
          label="Telefone"
          name="PHONE"
          class="lg:h-[84px] flex-2"
        >
          <UInput
            v-model="state.PHONE"
            v-maska="'(##) # ####-####'"
            placeholder="(00) 00000-0000"
            :maxlength="16"
            class="w-full"
            size="xl"
          />
        </UFormField>
  
        <div class="mt-6 flex-1">
          <UButton
            type="submit"
            block
            size="xl"
          >
            Cadastrar
          </UButton>
        </div>
      </div>
    </UForm>
  </ClientOnly>
</template>

<script setup>
import * as z from 'zod'

const config = useRuntimeConfig()

const schema = z.object({
  FNAME: z.string({ required_error: 'Obrigatório' }).min(2, 'Nome completo é obrigatório'),
  EMAIL: z.string({ required_error: 'Obrigatório' }).email('E-mail inválido'),
  PHONE: z.string({ required_error: 'Obrigatório' }).length(16, 'Telefone inválido')
})

const state = reactive({})

const toast = useToast()

const emailjs = ref({})

const showForm = computed(() => {
  return window && window.emailjs && config.public.emailJsUserId
})

async function onSubmit(event) {
  try {
    const data = {
      service_id: config.public.emailJsServiceId || '',
      template_id: 'template_t7arwgj',
      user_id: config.public.emailJsUserId || '',
      template_params: {
        'email': event.data.EMAIL,
        'name': event.data.FNAME,
        'phone': event.data.PHONE
      }
    };

    const response = await fetch('https://api.emailjs.com/api/v1.0/email/send', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data),
    })

    if (!response) {
      throw new Error('Erro ao enviar o formulário')
    }

    toast.add({
      title: 'Sucesso',
      description: `Solicitação enviada com sucesso! Verifique sua caixa de entrada e na lista de spam.`,
      color: 'success'
    })

    // Resetar o formulário
    state.FNAME = ''
    state.EMAIL = ''
    state.PHONE = ''
  } catch (error) {
    toast.add({
      title: 'Erro',
      description: 'Ocorreu um erro ao enviar o formulário.',
      color: 'error'
    })
    console.error(error)
  }
}

onMounted(() => {
  if (showForm.value) {
    emailjs.value = window.emailjs
  
    emailjs.value.init({
      publicKey: config.public.emailJsUserId || '',
    });
  }
})
</script>
