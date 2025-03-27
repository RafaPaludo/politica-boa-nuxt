<template>
  <form @submit.prevent="onSubmit">
    <UFormField
      name="email"
      label="Inscreva-se na nossa newsletter"
      size="xl"
    >
      <UInput
        v-model="email"
        type="email"
        class="w-full"
        placeholder="Seu melhor e-mail"
      >
        <template #trailing>
          <UButton
            type="submit"
            size="xs"
            label="Inscrever-se"
          />
        </template>
      </UInput>
    </UFormField>
  </form>
</template>

<script setup lang="ts">
const toast = useToast()

const email = ref('')

function onSubmit() {
  toast.add({
    title: 'Parabéns!',
    description: 'Você está inscrito na nossa newsletter.'
  })
}
</script>
