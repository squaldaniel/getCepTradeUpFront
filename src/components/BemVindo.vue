<script setup lang="ts">
import Paragrafo from './Paragrafo.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
</script>

<template>
  <Paragrafo>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Aplicação de Avaliação</template>

    Esta é uma aplicação de avaliaçãod previa de conhecimentos em desenvolvimento web
  </Paragrafo>
</template>
