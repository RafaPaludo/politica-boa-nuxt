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
          class="w-full"
          size="xl"
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
          class="w-full"
          size="xl"
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
          class="w-full"
          size="xl"
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

      <div class="flex gap-2">
        <UCheckbox class="items-center" v-model="state.CHECKBOX" label="Li e concordo com a"/>
        <ULink
          target="_blank"
          raw
          to="/politica-privacidade"
          active-class="font-bold"
          inactive-class="text-muted"
        >Política de Privacidade</ULink>
      </div>

      <div class="w-full mt-6">
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
</template>

<script setup lang="ts">
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

const formSuccess = defineModel()

const schema = z.object({
  FNAME: z.string({ required_error: 'Obrigatório' }).min(2, 'Nome completo é obrigatório'),
  EMAIL: z.string({ required_error: 'Obrigatório' }).email('E-mail inválido'),
  PHONE: z.string({ required_error: 'Obrigatório' }).length(16, 'Telefone inválido'),
  CHECKBOX: z.boolean({ required_error: 'Obrigatório' })
})

type Schema = z.output<typeof schema>

const state = reactive<Partial<Schema>>({})

const toast = useToast()

const props = defineProps<{
  tags?: string
}>()

async function onSubmit(event: FormSubmitEvent<Schema>) {
  try {
    // Enviar para o Mailchimp
    const formData = new FormData()
    formData.append('u', '35835573dce9ecd1ed104ac0a')
    formData.append('id', 'd5f4907ca7')
    formData.append('tags', props.tags || '')
    formData.append('FNAME', event.data.FNAME)
    formData.append('EMAIL', event.data.EMAIL)
    formData.append('PHONE', event.data.PHONE)

    // Campo anti-spam (honeypot)
    formData.append('b_35835573dce9ecd1ed104ac0a_d5f4907ca7', '')

    const response = await fetch('https://politicaboa.us4.list-manage.com/subscribe/post?u=35835573dce9ecd1ed104ac0a&amp;id=d5f4907ca7&amp;f_id=00d3b6e0f0', {
      method: 'POST',
      body: formData,
      mode: 'no-cors'
    })

    if (!response) {
      throw new Error('Erro ao enviar o formulário')
    }

    // Resetar o formulário
    state.FNAME = ''
    state.EMAIL = ''
    state.PHONE = ''
    state.CHECKBOX = false
    formSuccess.value = true;
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
  
