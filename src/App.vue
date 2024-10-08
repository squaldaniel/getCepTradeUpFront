<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Inicio from '@/components/Inicio.vue'
</script>

<template>
  <header>
    <img
      alt="Tradeup Logo"
      class="logo mt-[50px]"
      src="@/assets/logo-tradeup-rodape.png"
      width="125"
    />
    <div class="wrapper">
      <Inicio msg="Pesquisa de Cep" />
      <nav>
        <RouterLink class="btn btn-info flex items-center justify-center" to="/">Início</RouterLink>
        <RouterLink class="btn btn-success flex items-center justify-center py-2" to="/search">Pesquisar Cep´s</RouterLink>
      </nav>
    </div>
  </header>
  <RouterView />
</template>

<style scoped>

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}


nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
