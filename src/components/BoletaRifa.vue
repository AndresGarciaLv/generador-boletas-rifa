<template>
  <div class="boleta" :id="`boleta-${numero}`">
    <div class="boleta-header">
      <div class="header-decorative-top"></div>
      <div class="header-logo-container">
        <img src="/logos-stands/utcbis-logo.png" alt="UTCBIS Logo" class="header-logo" />
      </div>
      <h2>Boleta de Rifa</h2>
      <h3 class="header-subtitle">Evento de Videojuegos UT Cancún</h3>
      <div class="header-divider"></div>
      <div class="instrucciones-container">
        <p class="instrucciones">
          Participación válida una sola vez y por persona. Reúne todas las firmas en los stands de videojuegos y entrega esta boleta en la tómbola.
        </p>
        <p class="cierre">
          La recepción cierra a las <strong>4:30 pm</strong>
        </p>
      </div>
      <div class="header-decorative-bottom"></div>
    </div>

    <div class="boleta-info">
      <div class="folio-nombre">
        <span class="folio-label">Folio:</span>
        <span class="folio-numero">{{ folioFormateado }}</span>
      </div>
      <div class="folio-nombre">
        <span class="folio-label">Nombre:</span>
        <span class="nombre-linea">_____________________________</span>
      </div>
    </div>

    <div class="logos-grid">
      <div 
        v-for="(logo, index) in logos" 
        :key="index"
        class="logo-item"
      >
        <img 
          :src="getLogoPath(logo.archivo)" 
          :alt="logo.nombre"
          :class="{ 'logo-nestelia': logo.nombre === 'Nestelia' }"
          @error="handleImageError"
        />
      </div>
    </div>

    <div class="boleta-footer">
      
      <div class="footer-content">
        <p class="footer-titulo">CONSERVE ESTA PARTE</p>
        <div class="folio-destacado">
          <span class="folio-label-footer">FOLIO</span>
          <span class="folio-numero-footer">{{ folioFormateado }}</span>
        </div>
        <p class="footer-texto">Reúne todas las firmas en los stands de videojuegos y entrega esta boleta en la tómbola. / La recepción cierra a las 4:30 pm.</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  folio: {
    type: Number,
    required: true
  },
  numero: {
    type: Number,
    required: true
  }
})

const folioFormateado = computed(() => {
  return String(props.folio).padStart(3, '0')
})

// Mapeo de logos según el orden de la imagen
const logos = [
  { nombre: 'Nestelia', archivo: 'Logo-Nestelia-IDYGS101 (2).png' },
  { nombre: 'Kyrie Eleison - El Convento', archivo: 'KyrieEleison_ElConvento.png' },
  { nombre: 'Ice Cube Skull', archivo: 'ColdWar_Logo.jpeg' },
  { nombre: 'Purple Crystal', archivo: 'DREAMSHARDS - LOGO.jpg' },
  { nombre: 'Etaria', archivo: 'Etaria_Logo.png' },
  { nombre: 'Umbral - La Forja de las Almas', archivo: 'Umbral_Logo.png' },
  { nombre: 'Zombies Last Dawn', archivo: 'Zombies Last Dawn_Logo.png' },
  { nombre: 'Zoon', archivo: 'ZOON Logo.jpg' },
  { nombre: 'Hostis Aeternus', archivo: 'Hostis Aeternus.png' },
  { nombre: 'La Isla de Kapi', archivo: 'La isla de Kapi.jpeg' },
  { nombre: 'Antes del Sol y La Luna', archivo: 'LogoAntesDSolyLuna.png' },
  { nombre: 'The Last Farm', archivo: 'The Last Farm.png' },
  { nombre: 'Vampyr', archivo: 'VampyrROTNW.jpeg' },
  { nombre: 'Vivir o Morir', archivo: 'Vivir_O_Morir.png' },
  { nombre: 'Paw Print 2110', archivo: '2110LOGO.png' },
  { nombre: 'Anestesia', archivo: 'Anestesia.png' },
  { nombre: 'Banana Hellfire', archivo: 'BananaHellFire.png' },
  { nombre: 'Ex Tenebris', archivo: 'Extenebris.png' }
]

const getLogoPath = (archivo) => {
  return `/logos-stands/${archivo}`
}

const handleImageError = (event) => {
  console.warn(`Error cargando imagen: ${event.target.src}`)
  event.target.style.display = 'none'
}
</script>

<style scoped>
.boleta {
  width: 100%;
  max-width: 300px;
  margin: 0 auto;
  border: 1.5px solid #333;
  padding: 8px;
  background: white;
  font-family: Arial, sans-serif;
  page-break-inside: avoid;
}

.boleta-header {
  text-align: center;
  margin-bottom: 6px;
  padding: 6px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
}

.header-decorative-top,
.header-decorative-bottom {
  position: absolute;
  left: 0;
  right: 0;
  height: 3px;
  background: rgba(255, 255, 255, 0.3);
}

.header-decorative-top {
  top: 0;
}

.header-decorative-bottom {
  bottom: 0;
}

.header-logo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 3px;
}

.header-logo {
  max-width: 50px;
  max-height: 25px;
  height: auto;
  width: auto;
  object-fit: contain;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.3));
}

.boleta-header h2 {
  font-size: 12px;
  font-weight: 900;
  margin: 0 0 2px 0;
  color: #ffffff;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
  letter-spacing: 0.3px;
}

.header-subtitle {
  font-size: 8px;
  font-weight: 600;
  margin: 0 0 4px 0;
  color: #f0f0f0;
  letter-spacing: 0.2px;
}

.header-divider {
  width: 40px;
  height: 1.5px;
  background: #ffffff;
  margin: 0 auto 4px;
  border-radius: 2px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.instrucciones-container {
  background: rgba(255, 255, 255, 0.95);
  padding: 4px;
  border-radius: 3px;
  margin-top: 3px;
}

.instrucciones {
  font-size: 7px;
  margin: 2px 0;
  color: #333;
  line-height: 1.3;
  text-align: left;
}

.cierre {
  font-size: 7px;
  margin: 2px 0 0 0;
  color: #d63031;
  font-weight: bold;
  text-align: left;
}

.boleta-info {
  margin: 6px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.folio-nombre {
  display: flex;
  align-items: center;
  gap: 4px;
}

.folio-label {
  font-size: 8px;
  font-weight: bold;
}

.folio-numero {
  font-size: 9px;
  font-weight: bold;
  min-width: 25px;
}

.nombre-linea {
  font-size: 8px;
  min-width: 120px;
  display: inline-block;
  letter-spacing: 0.5px;
}

.logos-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(6, 1fr);
  gap: 0;
  margin: 0;
  min-height: 180px;
  border: 1.5px solid #333;
}

.logo-item {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  min-height: 30px;
  border: 0.5px solid #333;
}

.logo-item img {
  width: 55px;
  height: 55px;
  object-fit: contain;
  transform: scale(1.15);
}

.logo-item img.logo-nestelia {
  width: 65px;
  height: 65px;
  object-fit: contain;
  object-position: center;
  transform: scale(1.25);
}

.boleta-footer {
  margin-top: 8px;
  border: 1.5px dashed #ff6b6b;
  border-radius: 3px;
  background: linear-gradient(to bottom, #fff5f5 0%, #ffe0e0 100%);
  padding: 6px;
  position: relative;
}

.linea-punteada-footer {
  border-top: 2px dashed #ff6b6b;
  margin: 0 0 12px 0;
  position: relative;
}


.footer-content {
  text-align: center;
}

.footer-titulo {
  font-size: 7px;
  font-weight: bold;
  color: #d63031;
  margin-bottom: 4px;
  text-transform: uppercase;
  letter-spacing: 0.3px;
}

.folio-destacado {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 6px 0;
  padding: 5px;
  background: white;
  border: 1.5px solid #d63031;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(214, 48, 49, 0.2);
}

.folio-label-footer {
  font-size: 6px;
  font-weight: bold;
  color: #d63031;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 2px;
}

.folio-numero-footer {
  font-size: 20px;
  font-weight: 900;
  color: #d63031;
  line-height: 1;
  letter-spacing: 1px;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
}

.footer-texto {
  font-size: 7px;
  color: #000;
  line-height: 1.3;
  margin: 4px 0 0 0;
}

@media print {
  .boleta {
    page-break-inside: avoid;
    margin-bottom: 20px;
  }
}
</style>

