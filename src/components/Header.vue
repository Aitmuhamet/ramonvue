<script setup>
import Offcanvas from './Offcanvas.vue';
</script>

<template>
  <header class="header">
    <nav>
      <a href="https://rickandmortyapi.com/" class="icon">
        <img src="../assets/img/icon.png" alt="">
      </a>
    </nav>
  </header>
</template>

<style scoped>
nav {
  padding: 0 1.5rem;
  min-height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.icon {
  margin-right: auto;
}

.icon img {
  width: 40px;
  height: 40px;
}
</style>
