<template>
  <div class="app-container">
    <header class="header">
      <h1>Guia de Aventura</h1>
    </header>

    <main class="conteudo">
      <Mapa />
      <Bussola />
    </main>
  </div>
</template>

<script setup>
import Mapa from './components/Mapa.vue';
import Bussola from './components/Bussola.vue';
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

body {
  background-color: #e9ecef;
}

.app-container {
  max-width: 480px;
  margin: 0 auto;
  min-height: 100vh;
  background-color: #ffffff;
  display: flex;
  flex-direction: column;
}

.header {
  background-color: #007bff;
  color: white;
  text-align: center;
  padding: 14px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.header h1 {
  font-size: 1.3rem;
}

.conteudo {
  flex: 1;
  display: flex;
  flex-direction: column;
}
</style>