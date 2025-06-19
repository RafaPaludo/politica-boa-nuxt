<template>
  <UForm
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
        name="MMERGE7"
        class="w-full"
      >
        <UTextarea
          v-model="state.MMERGE7"
          resize
          class="w-full"
        />
      </UFormField>

      <!-- Campo anti-spam (honeypot) - escondido -->
      <div
        style="position: absolute; left: -5000px;"
        aria-hidden="true"
      >
        <input
          type="text"
          name="b_35835573dce9ecd1ed104ac0a_d5f4907ca7"
          tabindex="-1"
          value=""
        >
      </div>

      <UButton
        type="submit"
        block
        size="xl"
      >
        Enviar
      </UButton>
    </div>
  </UForm>
</template>

<script setup lang="ts">
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

const schema = z.object({
  FNAME: z.string({ required_error: 'Obrigatório' }).min(2, 'Nome completo é obrigatório'),
  EMAIL: z.string({ required_error: 'Obrigatório' }).email('E-mail inválido'),
  PHONE: z.string({ required_error: 'Obrigatório' }).length(16, 'Telefone inválido'),
  MMERGE7: z.string().optional()
})

type Schema = z.output<typeof schema>

const state = reactive<Partial<Schema>>({})

const toast = useToast()

async function onSubmit(event: FormSubmitEvent<Schema>) {
  try {
    // Enviar para o Mailchimp
    const formData = new FormData()
    formData.append('u', '35835573dce9ecd1ed104ac0a')
    formData.append('id', 'd5f4907ca7')
    formData.append('FNAME', event.data.FNAME)
    formData.append('EMAIL', event.data.EMAIL)
    formData.append('PHONE', event.data.PHONE)
    if (event.data.MMERGE7) formData.append('MMERGE7', event.data.MMERGE7)

    // Campo anti-spam (honeypot)
    formData.append('b_35835573dce9ecd1ed104ac0a_d5f4907ca7', '')

    const response = await fetch('https://politicaboa.us4.list-manage.com/subscribe/post?u=35835573dce9ecd1ed104ac0a&amp;id=d5f4907ca7&amp;f_id=002eb1e0f0', {
      method: 'POST',
      body: formData,
      mode: 'no-cors'
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
    state.MMERGE7 = ''
  } catch (error) {
    toast.add({
      title: 'Erro',
      description: 'Ocorreu um erro ao enviar o formulário.',
      color: 'error'
    })
    console.error(error)
  }
}
</script>
