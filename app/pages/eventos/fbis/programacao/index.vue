<template>
  <UContainer>
    <UPageHeader
      title="1º Fórum Brasileiro de Inovação Sindical"
      description="O FBIS 2025 - Fórum Brasileiro de Inovação Sindical chega em sua primeira edição com uma proposta ousada e necessária: repensar o papel, as estratégias e os caminhos para o futuro do sindicalismo no Brasil. "
    >
      <template #headline>
        <UBadge
          label="Programação"
          variant="subtle"
        />
      </template>
    </UPageHeader>

    <UPage>
      <UPageBody>
        <img
          v-if="programacaoFBIS"
          :src="programacaoFBIS"
          alt="Programação 1º FBIS"
          class="w-full h-auto object-cover rounded-lg shadow-lg"
        >
      </UPageBody>
    </UPage>
  </UContainer>
</template>

<script setup lang="ts">
import programacaoFBIS from '~/assets/img/eventos/fbis/programacao.jpg'
</script>