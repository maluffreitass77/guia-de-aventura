<template>
  <div class="home-container">
    <div class="hero-banner">
      <h2>Bem-vindo ao Guia de Aventura! 🗺️</h2>
      <p>Explore a cidade, descubra pontos turísticos e oriente-se em tempo real.</p>
    </div>

    <div class="card-status">
      <h3> Status do Dispositivo</h3>
      <div class="item-status">
        <span>GPS & Geolocalização:</span>
        <span class="tag-status ok">Ativo</span>
      </div>
      <div class="item-status">
        <span>Sensores de Orientação:</span>
        <span class="tag-status ok">Conectado</span>
      </div>
    </div>

    <div class="atalhos">
      <h3>Acesso Rápido</h3>
      <div class="grid-atalhos">
        <button class="btn-atalho" @click="$emit('mudarTela', 'mapa')">
          <span class="icone">📍</span>
          <span>Ver Mapa</span>
        </button>
        <button class="btn-atalho" @click="$emit('mudarTela', 'bussola')">
          <span class="icone">🧭</span>
          <span>Bússola</span>
        </button>
        <button class="btn-atalho" @click="$emit('mudarTela', 'perto')">
          <span class="icone">🎯</span>
          <span>Perto de Mim</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineEmits(['mudarTela']);
</script>

<style scoped>
.home-container {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.hero-banner {
  background: linear-gradient(135deg, #007bff, #0056b3);
  color: white;
  padding: 20px;
  border-radius: 12px;
}
.hero-banner h2 {
  font-size: 1.3rem;
  margin-bottom: 6px;
}
.hero-banner p {
  font-size: 0.9rem;
  opacity: 0.9;
}
.card-status {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 15px;
  border: 1px solid #e9ecef;
}
.card-status h3 {
  font-size: 1rem;
  margin-bottom: 10px;
}
.item-status {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  font-size: 0.85rem;
}
.tag-status.ok {
  background-color: #d4edda;
  color: #155724;
  padding: 2px 8px;
  border-radius: 6px;
  font-weight: bold;
}
.grid-atalhos {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-top: 10px;
}
.btn-atalho {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: white;
  border: 1px solid #dee2e6;
  padding: 15px 5px;
  border-radius: 10px;
  cursor: pointer;
  min-height: 80px;
}
.btn-atalho .icone {
  font-size: 1.5rem;
  margin-bottom: 4px;
}
.btn-atalho span {
  font-size: 0.8rem;
  font-weight: bold;
}
</style>