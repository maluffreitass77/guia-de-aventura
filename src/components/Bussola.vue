<template>
  <div class="bussola-card">
    <h3 class="titulo">Bússola</h3>
    
    <!-- Exigência 3.b: Exibir pontos cardeais N-Norte, S-Sul, L-Leste, O-Oeste -->
    <div class="direcao-texto">{{ direcaoCardinal }} ({{ graus.toFixed(0) }}°)</div>

    <div class="disco-bussola" :style="{ transform: `rotate(${-graus}deg)` }">
      <span class="ponto norte">N</span>
      <span class="ponto leste">L</span>
      <span class="ponto sul">S</span>
      <span class="ponto oeste">O</span>
      <div class="agulha"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { Motion } from '@capacitor/motion';

const graus = ref(0);
let motionListener = null;

// Mapeamento dos ângulos para os pontos cardeais
const direcaoCardinal = computed(() => {
  const g = graus.value;
  if (g >= 315 || g < 45) return 'N - Norte';
  if (g >= 45 && g < 135) return 'L - Leste';
  if (g >= 135 && g < 225) return 'S - Sul';
  if (g >= 225 && g < 315) return 'O - Oeste';
  return 'N - Norte';
});

onMounted(async () => {
  try {
    // Exigência 3.a: Utilizar sensores disponíveis no dispositivo (Orientação)
    motionListener = await Motion.addListener('orientation', (event) => {
      if (event.alpha !== null && event.alpha !== undefined) {
        graus.value = event.alpha;
      }
    });
  } catch (error) {
    console.error("Sensor de orientação não suportado no navegador de testes.", error);
  }
});

onUnmounted(() => {
  if (motionListener) {
    motionListener.remove();
  }
});
</script>

<style scoped>
.bussola-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #f8f9fa;
  padding: 15px;
  margin: 10px;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.titulo {
  font-size: 1.1rem;
  margin-bottom: 4px;
  color: #333;
}

.direcao-texto {
  font-size: 1rem;
  font-weight: bold;
  color: #007bff;
  margin-bottom: 10px;
}

.disco-bussola {
  position: relative;
  width: 120px;
  height: 120px;
  border: 4px solid #333;
  border-radius: 50%;
  background-color: #fff;
  transition: transform 0.15s ease-out;
}

.ponto {
  position: absolute;
  font-weight: bold;
  font-size: 0.9rem;
}

.norte { top: 4px; left: 50%; transform: translateX(-50%); color: #dc3545; }
.sul { bottom: 4px; left: 50%; transform: translateX(-50%); color: #333; }
.leste { right: 6px; top: 50%; transform: translateY(-50%); color: #333; }
.oeste { left: 6px; top: 50%; transform: translateY(-50%); color: #333; }

.agulha {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 4px;
  height: 48px;
  background: linear-gradient(to bottom, #dc3545 50%, #333 50%);
  transform: translate(-50%, -50%);
  border-radius: 2px;
}
</style>