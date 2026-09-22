<template>
  <div class="app-mobile">
    <!-- Cabeçalho Estilizado -->
    <header class="app-bar">
      <div class="brand">
        <span class="icon">🧭</span>
        <h1>Guia de Aventura</h1>
      </div>
    </header>

    <!-- Conteúdo Scrollável -->
    <main class="app-body">
      <Home v-if="telaAtiva === 'home'" @mudarTela="trocarTela" />
      
      <div v-show="telaAtiva === 'mapa'" class="aba-mapa">
        <Mapa />
      </div>

      <div v-if="telaAtiva === 'bussola'" class="aba-bussola">
        <Bussola />
      </div>

      <PertoDeMim v-if="telaAtiva === 'perto'" />
    </main>

    <!-- Barra de Navegação Inferior Nativa -->
    <nav class="bottom-nav">
      <button 
        :class="['nav-btn', { active: telaAtiva === 'home' }]" 
        @click="trocarTela('home')"
      >
        <span class="nav-icon">🏠</span>
        <span class="nav-label">Início</span>
      </button>

      <button 
        :class="['nav-btn', { active: telaAtiva === 'mapa' }]" 
        @click="trocarTela('mapa')"
      >
        <span class="nav-icon">🗺️</span>
        <span class="nav-label">Mapa</span>
      </button>

      <button 
        :class="['nav-btn', { active: telaAtiva === 'bussola' }]" 
        @click="trocarTela('bussola')"
      >
        <span class="nav-icon">🧭</span>
        <span class="nav-label">Bússola</span>
      </button>

      <button 
        :class="['nav-btn', { active: telaAtiva === 'perto' }]" 
        @click="trocarTela('perto')"
      >
        <span class="nav-icon">🎯</span>
        <span class="nav-label">Perto</span>
      </button>
    </nav>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Home from './components/Home.vue';
import Mapa from './components/Mapa.vue';
import Bussola from './components/Bussola.vue';
import PertoDeMim from './components/PertoDemim.vue';

const telaAtiva = ref('mapa');

const trocarTela = (nomeTela) => {
  telaAtiva.value = nomeTela;
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  -webkit-tap-highlight-color: transparent;
}

html, body {
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-color: #f4f6f8;
}

#app {
  width: 100%;
  height: 100%;
  min-height: 0;
  overflow: hidden;
}

/* Ocupa 100% da tela do celular sem sobras laterais */
.app-mobile {
  width: 100vw;
  height: 100%;
  height: 100dvh;
  max-height: 100dvh;
  min-height: 0;
  display: flex;
  flex-direction: column;
  background-color: #ffffff;
}

.app-bar {
  background: linear-gradient(135deg, #0d6efd, #0a58ca);
  color: white;
  padding: max(16px, env(safe-area-inset-top)) 20px 16px;
  flex: 0 0 auto;
  box-shadow: 0 2px 10px rgba(0,0,0,0.15);
  z-index: 10;
}

.brand {
  display: flex;
  align-items: center;
  gap: 10px;
}

.brand .icon {
  font-size: 1.4rem;
}

.brand h1 {
  font-size: 1.2rem;
  font-weight: 700;
  letter-spacing: 0.5px;
}

.app-body {
  flex: 1;
  min-height: 0;
  overflow-y: auto;
  position: relative;
}

.aba-mapa, .aba-bussola {
  width: 100%;
  height: 100%;
  min-height: 0;
}

/* Barra de Navegação estilo Mobile App */
.bottom-nav {
  display: flex;
  background-color: #ffffff;
  border-top: 1px solid #e9ecef;
  height: calc(65px + env(safe-area-inset-bottom));
  min-height: calc(65px + env(safe-area-inset-bottom));
  padding-bottom: max(5px, env(safe-area-inset-bottom));
  flex: 0 0 auto;
  box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
}

.nav-btn {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: none;
  border: none;
  color: #6c757d;
  cursor: pointer;
  transition: all 0.2s ease;
}

.nav-icon {
  font-size: 1.3rem;
  margin-bottom: 2px;
}

.nav-label {
  font-size: 0.75rem;
  font-weight: 600;
}

.nav-btn.active {
  color: #0d6efd;
}

.nav-btn.active .nav-icon {
  transform: translateY(-2px);
}
</style>