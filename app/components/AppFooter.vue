<script setup lang="ts">
const columns = [{
  label: 'Recursos',
  children: [{
    label: 'Help center'
  }]
}, {
  label: 'Sobre',
  children: [{
    label: 'Afiliados'
  }]
}, {
  label: 'Companhia',
  children: [{
    label: 'Blog'
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
        <UFooterColumns :columns="columns">
          <template #right>
            <SubscribeForm />
          </template>
        </UFooterColumns>
      </UContainer>
    </template>

    <template #left>
      <p class="text-(--ui-text-muted) text-sm">
        Copyright © {{ new Date().getFullYear() }}. Todos os direitos reservados.
      </p>
    </template>
  </UFooter>
</template>
