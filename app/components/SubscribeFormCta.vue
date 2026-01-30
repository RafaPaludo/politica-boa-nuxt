<template>
  <ClientOnly>
    <UForm
      v-if="showForm"
      :state="state"
      :schema="schema"
      class=""
      @submit="onSubmit"
    >
      <div class="flex flex-col lg:items-start gap-2">
        <UFormField
          label="Nome completo"
          name="FNAME"
          class="w-full"
        >
          <UInput
            v-model="state.FNAME"
            placeholder="Seu nome completo"
            size="xl"
            class="w-full"
          />
        </UFormField>

        <UFormField
          label="Endereço de e-mail"
          name="EMAIL"
          class="w-full"
        >
          <UInput
            v-model="state.EMAIL"
            type="email"
            placeholder="seu@email.com"
            size="xl"
            class="w-full"
          />
        </UFormField>

        <UFormField
          label="Telefone"
          name="PHONE"
          class="w-full"
        >
          <UInput
            v-model="state.PHONE"
            v-maska="'(##) # ####-####'"
            placeholder="(00) 00000-0000"
            :maxlength="16"
            size="xl"
            class="w-full"
          />
        </UFormField>

        <UFormField
          label="Mensagem"
          name="MESSAGE"
          class="w-full"
        >
          <UTextarea
            v-model="state.MESSAGE"
            resize
            class="w-full"
          />
        </UFormField>

        <UButton
          type="submit"
          block
          size="xl"
          class="mt-3"
        >
          Enviar
        </UButton>
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
  PHONE: z.string({ required_error: 'Obrigatório' }).length(16, 'Telefone inválido'),
  MESSAGE: z.string().optional()
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
      template_id: 'template_p0fmujh',
      user_id: config.public.emailJsUserId || '',
      template_params: {
        'email': event.data.EMAIL,
        'name': event.data.FNAME,
        'phone': event.data.PHONE,
        'message': event.data.MESSAGE,
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
      description: `Mensagem enviada com sucesso!`,
      color: 'success'
    })

    // Resetar o formulário
    state.FNAME = ''
    state.EMAIL = ''
    state.PHONE = ''
    state.MESSAGE = ''
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
