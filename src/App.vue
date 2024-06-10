<script setup>

import Header from './components/Header.vue'
import Hero from './components/Hero.vue'
import Characters from './components/Characters.vue'
import Pagination from './components/Pagination.vue'

</script>

<template>
  <Header />
  <Hero />
  <main class="main">
    <Characters />
  </main>


</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
  align-items: center;
  padding: 4.5rem 0px;
  background: rgb(39, 43, 51);
  min-height: calc(-60px + 50vh);
}
@media (max-width: 576px) {
  main {
    padding: 1.5rem 0px;
  }
}
</style>
