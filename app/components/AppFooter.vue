<script setup lang="ts">
const columns = [{
  label: 'Fale com a gente',
  children: [
    {
      label: 'Whatsapp',
      to: 'https://wa.link/w5dy99',
      target: '_blank'
    },
    {
      label: 'Telefone',
      to: 'tel:+55 48 99121-8168',
      target: '_blank'
    },
    {
      label: 'Email',
      to: 'mailto:contato@politicaboa.com.br?subject=Contato&body=Olá, gostaria de mais informações sobre o IPB',
      target: '_self'
    }
  ]
}, {
  label: 'Cursos',
  children: [{
    label: 'Afiliados'
  }]
}, {
  label: 'Conteúdos',
  children: [{
    label: 'Blog',
    to: '/blog',
    target: '_self'
  }]
}]
</script>

<template>
  <USeparator
    label="IPB"
    class="h-px"
  />
  <UFooter :ui="{ top: 'border-b border-[var(--ui-border)]' }">
    <template #top>
      <UContainer>
        <UFooterColumns :columns="columns" />
      </UContainer>
    </template>

    <template #left>
      <p class="text-(--ui-text-muted) text-sm">
        Copyright © {{ new Date().getFullYear() }}. Todos os direitos reservados.
      </p>
    </template>
  </UFooter>
</template>
