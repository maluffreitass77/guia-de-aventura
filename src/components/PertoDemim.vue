<template>
  <div class="perto-container">
    <h2>Perto de Mim</h2>
    <p class="sub">Filtre pontos turísticos em um raio próximo de você</p>

    <div class="filtro-box">
      <label for="raio">Raio máximo: <strong>{{ raioMax }} km</strong></label>
      <input 
        id="raio" 
        type="range" 
        min="0.5" 
        max="5" 
        step="0.5" 
        v-model.number="raioMax"
      />
    </div>

    <div class="lista-pontos">
      <div 
        v-for="ponto in pontosFiltrados" 
        :key="ponto.id" 
        class="card-ponto"
      >
        <img :src="ponto.imagem" :alt="ponto.nome" />
        <div class="ponto-info">
          <h3>{{ ponto.nome }}</h3>
          <p>{{ ponto.descricao }}</p>
          <span class="badge">{{ ponto.distancia }} km de distância</span>
        </div>
      </div>

      <div v-if="pontosFiltrados.length === 0" class="sem-resultados">
        Nenhum ponto encontrado neste raio. Tente aumentar a distância!
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const raioMax = ref(2.0);

const pontosTuristicos = [
  {
    id: 1,
    nome: "Parque Central",
    descricao: "Área verde exuberante para passeios e caminhadas.",
    imagem: "https://picsum.photos/400/200?random=1",
    distancia: 1.2
  },
  {
    id: 2,
    nome: "Mirante da Cidade",
    descricao: "Vista panorâmica espetacular de toda a região.",
    imagem: "https://picsum.photos/400/200?random=2",
    distancia: 2.5
  },
  {
    id: 3,
    nome: "Museu Histórico",
    descricao: "Exposição cultural e história da fundação local.",
    imagem: "https://picsum.photos/400/200?random=3",
    distancia: 0.8
  }
];

const pontosFiltrados = computed(() => {
  return pontosTuristicos.filter(p => p.distancia <= raioMax.value);
});
</script>

<style scoped>
.perto-container {
  padding: 16px;
}
.sub {
  font-size: 0.85rem;
  color: #666;
  margin-bottom: 15px;
}
.filtro-box {
  background: #f1f3f5;
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}
input[type="range"] {
  width: 100%;
}
.lista-pontos {
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.card-ponto {
  display: flex;
  gap: 12px;
  background: #fff;
  border: 1px solid #e9ecef;
  border-radius: 10px;
  padding: 10px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
}
.card-ponto img {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 8px;
}
.ponto-info h3 {
  font-size: 1rem;
  margin-bottom: 4px;
}
.ponto-info p {
  font-size: 0.8rem;
  color: #555;
  margin-bottom: 6px;
}
.badge {
  background-color: #e7f5ff;
  color: #1c7ed6;
  font-size: 0.75rem;
  padding: 3px 8px;
  border-radius: 12px;
  font-weight: bold;
}
.sem-resultados {
  text-align: center;
  color: #888;
  padding: 20px;
}
</style>