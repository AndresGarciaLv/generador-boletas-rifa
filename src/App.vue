<template>
  <div class="app-container">
    <div class="controls">
      <h1>Boletas de Rifa - Evento de Videojuegos UT Cancún</h1>
      <div class="controls-panel">
        <label>
          Folio inicial:
          <input type="number" v-model.number="folioInicial" min="1" />
        </label>
        <label>
          Folio final:
          <input type="number" v-model.number="folioFinal" :min="folioInicial" />
        </label>
        <div class="info-paginas">
          <span v-if="totalBoletas > 0">
            Total: {{ totalBoletas }} boleta{{ totalBoletas !== 1 ? 's' : '' }} 
            ({{ totalPaginas }} página{{ totalPaginas !== 1 ? 's' : '' }})
          </span>
        </div>
        <button @click="generarPDF" class="btn-download" :disabled="generandoPDF">
          <span v-if="generandoPDF">Generando... {{ progresoPDF }}%</span>
          <span v-else>Descargar PDF</span>
        </button>
      </div>
    </div>

    <div id="boletas-container" class="boletas-container">
      <div 
        v-for="(pagina, indexPagina) in paginas" 
        :key="indexPagina"
        class="pagina"
      >
        <BoletaRifa
          v-for="(folio, index) in pagina"
          :key="folio"
          :folio="folio"
          :numero="folio"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import BoletaRifa from './components/BoletaRifa.vue'
import jsPDF from 'jspdf'
import html2canvas from 'html2canvas'

const folioInicial = ref(1)
const folioFinal = ref(3)
const generandoPDF = ref(false)
const progresoPDF = ref(0)

const totalBoletas = computed(() => {
  if (folioFinal.value < folioInicial.value) return 0
  return folioFinal.value - folioInicial.value + 1
})

const totalPaginas = computed(() => {
  return Math.ceil(totalBoletas.value / 3)
})

const paginas = computed(() => {
  const paginasArray = []
  let folioActual = folioInicial.value
  const total = totalBoletas.value
  
  if (total <= 0) return []
  
  const numeroPaginas = totalPaginas.value
  
  for (let i = 0; i < numeroPaginas; i++) {
    const boletasEnPagina = []
    for (let j = 0; j < 3; j++) {
      if (folioActual <= folioFinal.value) {
        boletasEnPagina.push(folioActual)
        folioActual++
      }
    }
    if (boletasEnPagina.length > 0) {
      paginasArray.push(boletasEnPagina)
    }
  }
  
  return paginasArray
})

const generarPDF = async () => {
  generandoPDF.value = true
  progresoPDF.value = 0
  
  try {
    const container = document.getElementById('boletas-container')
    const paginas = container.querySelectorAll('.pagina')
    const totalPaginas = paginas.length
    
    // Usar dimensiones que funcionen tanto para A4 como Carta
    // A4 landscape: 297mm × 210mm
    // Carta landscape: 279mm × 216mm
    // Usamos las dimensiones más pequeñas para asegurar compatibilidad
    const pdfWidth = 279 // Carta width (más pequeño), funcionará en A4 también
    const pdfHeight = 210 // A4 height (más pequeño), funcionará en Carta también
    
    const pdf = new jsPDF({
      orientation: 'landscape',
      unit: 'mm',
      format: [pdfWidth, pdfHeight]
    })
    
    for (let i = 0; i < paginas.length; i++) {
      if (i > 0) {
        pdf.addPage()
      }
      
      // Actualizar progreso
      progresoPDF.value = Math.round(((i + 1) / totalPaginas) * 100)
      
      // Optimizar html2canvas: reducir scale y agregar opciones de rendimiento
      const canvas = await html2canvas(paginas[i], {
        scale: 1.5, // Reducido de 2 a 1.5 para mejor rendimiento (suficiente calidad)
        useCORS: true,
        logging: false,
        removeContainer: true,
        backgroundColor: '#ffffff',
        windowWidth: paginas[i].scrollWidth,
        windowHeight: paginas[i].scrollHeight
      })
      
      const imgData = canvas.toDataURL('image/png', 0.92) // Comprimir ligeramente PNG
      
      // Márgenes para evitar que se corte al imprimir (reducidos un poco)
      const margin = 8 // mm en cada lado
      const availableWidth = pdfWidth - (margin * 2)
      const availableHeight = pdfHeight - (margin * 2)
      
      // Calcular dimensiones de la imagen manteniendo proporción dentro del área disponible
      let imgWidth = availableWidth
      let imgHeight = (canvas.height * imgWidth) / canvas.width
      
      // Si la altura es mayor que el alto disponible, ajustar por altura
      if (imgHeight > availableHeight) {
        imgHeight = availableHeight
        imgWidth = (canvas.width * imgHeight) / canvas.height
      }
      
      // Centrar la imagen en el área disponible (con márgenes)
      const x = margin + (availableWidth - imgWidth) / 2
      const y = margin + (availableHeight - imgHeight) / 2
      
      pdf.addImage(imgData, 'PNG', x, y, imgWidth, imgHeight)
      
      // Pequeño delay para no bloquear el navegador
      if (i < paginas.length - 1) {
        await new Promise(resolve => setTimeout(resolve, 10))
      }
    }
    
    pdf.save(`boletas-rifa-folio-${folioInicial.value}-${folioFinal.value}.pdf`)
  } catch (error) {
    console.error('Error al generar PDF:', error)
    alert('Error al generar el PDF. Por favor, intenta de nuevo.')
  } finally {
    generandoPDF.value = false
    progresoPDF.value = 0
  }
}
</script>

<style scoped>
.app-container {
  width: 100%;
}

.controls {
  background: white;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.controls h1 {
  margin-bottom: 20px;
  color: #333;
  font-size: 24px;
}

.controls-panel {
  display: flex;
  gap: 20px;
  align-items: center;
  flex-wrap: wrap;
}

.controls-panel label {
  display: flex;
  flex-direction: column;
  gap: 5px;
  font-weight: bold;
  color: #555;
}

.controls-panel input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.info-paginas {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  background-color: #e3f2fd;
  border-radius: 4px;
  color: #1976d2;
  font-weight: 500;
  font-size: 14px;
}

.btn-download {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  transition: background-color 0.3s;
}

.btn-download:hover:not(:disabled) {
  background-color: #45a049;
}

.btn-download:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
  opacity: 0.7;
}

.boletas-container {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagina {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 10px;
  margin: 0 auto 20px auto;
  page-break-after: always;
  justify-content: center;
  max-width: 100%;
}

.pagina:last-child {
  margin-bottom: 0;
}
</style>

