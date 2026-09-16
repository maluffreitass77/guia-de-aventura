<template>
  <div class="mapa-wrapper">
    <!-- Painel com Coordenadas Atuais do Usuário (Exigência 1.b / 1.c) -->
    <div class="painel-coordenadas">
      <p><strong>Minha Posição Atual:</strong></p>
      <div v-if="userCoords.lat">
        <span>Lat: {{ userCoords.lat.toFixed(6) }}</span> | 
        <span>Lng: {{ userCoords.lng.toFixed(6) }}</span>
      </div>
      <div v-else class="status-carregando">Obtendo localização GPS...</div>
    </div>

    <!-- Container do Mapa (Exigência 2.a / 2.b / 2.c) -->
    <div id="map"></div>

    <!-- Modal do Ponto Turístico Selecionado (Exigência 4.a / 5.a / 6.b) -->
    <div v-if="pontoSelecionado" class="card-detalhes">
      <img :src="pontoSelecionado.imagem" :alt="pontoSelecionado.nome" class="img-ponto" />
      <h3>{{ pontoSelecionado.nome }}</h3>
      <p class="descricao">{{ pontoSelecionado.descricao }}</p>
      <p class="distancia"><strong>Distância aprox.:</strong> {{ pontoSelecionado.distancia }} km</p>

      <div class="acoes">
        <!-- Opção Como Chegar (Exigência 5.a / 5.b) -->
        <button class="btn btn-navegar" @click="comoChegar(pontoSelecionado)">
          📍 Como chegar
        </button>
        <button class="btn btn-fechar" @click="pontoSelecionado = null">
          Fechar
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, onUnmounted } from 'vue';
import { Geolocation } from '@capacitor/geolocation';
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

// Fix dos ícones padrão do Leaflet no Webpack/Vite
import iconUrl from 'leaflet/dist/images/marker-icon.png';
import iconRetinaUrl from 'leaflet/dist/images/marker-icon-2x.png';
import shadowUrl from 'leaflet/dist/images/marker-shadow.png';

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconUrl,
  iconRetinaUrl,
  shadowUrl,
});

const pontoSelecionado = ref(null);
const userCoords = ref({ lat: null, lng: null });
let map = null;
let userMarker = null;
let watchId = null;

// Exigência 2.c: Pelo menos 3 pontos de interesse no mapa
const pontosTuristicos = [
  {
    id: 1,
    nome: "Parque Central",
    descricao: "Área verde exuberante ideal para passeios ao ar livre, caminhadas e piqueniques com a família.",
    lat: -26.3045,
    lng: -48.8487,
    imagem: "https://picsum.photos/400/200?random=1",
    distancia: "1.2"
  },
  {
    id: 2,
    nome: "Mirante da Cidade",
    descricao: "Ponto elevado com vista panorâmica espetacular de toda a região urbana e serrana.",
    lat: -26.3100,
    lng: -48.8450,
    imagem: "https://picsum.photos/400/200?random=2",
    distancia: "2.5"
  },
  {
    id: 3,
    nome: "Museu Histórico",
    descricao: "Exposição cultural com documentos, artefatos antigos e acervo sobre a fundação local.",
    lat: -26.3000,
    lng: -48.8500,
    imagem: "https://picsum.photos/400/200?random=3",
    distancia: "0.8"
  }
];

onMounted(async () => {
  // Inicializa o mapa com centro inicial genérico
  map = L.map('map').setView([-26.3045, -48.8487], 14);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap'
  }).addTo(map);

  // Adiciona os 3 pontos de interesse no mapa (Exigência 2.c)
  pontosTuristicos.forEach(ponto => {
    const marker = L.marker([ponto.lat, ponto.lng]).addTo(map);
    // Ao clicar no ponto, exibe detalhes (Exigência 4.a)
    marker.on('click', () => {
      pontoSelecionado.value = ponto;
    });
  });

  // Exigência 1.a: Solicitar permissão de localização
  await iniciarGeolocalizacao();
});

const iniciarGeolocalizacao = async () => {
  try {
    const perm = await Geolocation.requestPermissions();
    if (perm.location === 'granted' || perm.coarseLocation === 'granted') {
      // Exigência 1.c: Atualizar a posição conforme o usuário se movimenta
      watchId = await Geolocation.watchPosition(
        { enableHighAccuracy: true, timeout: 10000 },
        (position, err) => {
          if (err || !position) return;
          
          const { latitude, longitude } = position.coords;
          userCoords.value = { lat: latitude, lng: longitude };

          // Marcador do Usuário
          if (!userMarker) {
            userMarker = L.circleMarker([latitude, longitude], {
              color: '#007bff',
              fillColor: '#007bff',
              fillOpacity: 0.8,
              radius: 9
            }).addTo(map).bindPopup("Sua Localização Atual");
            
            map.setView([latitude, longitude], 14);
          } else {
            userMarker.setLatLng([latitude, longitude]);
          }
        }
      );
    }
  } catch (error) {
    console.error("Erro ao acessar geolocalização:", error);
  }
};

// Exigência 5.a / 5.b: Navegação "Como chegar" integrando serviço de mapas
const comoChegar = (ponto) => {
  const url = `https://www.google.com/maps/dir/?api=1&destination=${ponto.lat},${ponto.lng}`;
  window.open(url, '_system');
};

onUnmounted(() => {
  if (watchId !== null) {
    Geolocation.clearWatch({ id: watchId });
  }
});
</script>

<style scoped>
.mapa-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.painel-coordenadas {
  background-color: #1a252f;
  color: #ecf0f1;
  padding: 10px;
  font-size: 0.8rem;
  text-align: center;
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
  z-index: 5;
}

#map {
  flex: 1;
  width: 100%;
  height: 100%;
}

.card-detalhes {
  position: absolute;
  bottom: 20px;
  left: 15px;
  right: 15px;
  background: #ffffff;
  padding: 16px;
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);
  z-index: 1000;
  color: #2c3e50;
}

.img-ponto {
  width: 100%;
  height: 120px;
  object-fit: cover;
  border-radius: 10px;
  margin-bottom: 8px;
}

.descricao {
  font-size: 0.85rem;
  color: #7f8c8d;
  margin: 4px 0 8px 0;
}

.distancia {
  font-size: 0.85rem;
  margin-bottom: 12px;
}

.acoes {
  display: flex;
  gap: 10px;
}

.btn {
  flex: 1;
  min-height: 48px;
  font-size: 0.95rem;
  font-weight: bold;
  border: none;
  border-radius: 10px;
  cursor: pointer;
}

.btn-navegar {
  background-color: #27ae60;
  color: white;
}

.btn-fechar {
  background-color: #e74c3c;
  color: white;
}
</style>