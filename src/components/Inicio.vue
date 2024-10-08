<script setup lang="ts">
defineProps<{
  msg: string
}>()
</script>

<template>
  <div>
    <h1 class="mb-5 text-5xl font-bold text-white">{{ msg }}</h1>
    <h3 class="text-white">
      Pesquise qualquer CEP brasileiro. Clique quem pesquisar <br/>CEP´s abaixo.
    </h3>
  </div>
</template>