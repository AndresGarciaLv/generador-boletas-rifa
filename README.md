# Generador de Boletas de Rifa - Evento de Videojuegos UT Cancún

Aplicación web desarrollada para generar y descargar boletas de rifa en formato PDF para el evento de videojuegos de la Universidad Tecnológica de Cancún (UT Cancún).

## 📋 Descripción

Este generador permite crear boletas de rifa personalizadas con formato profesional que incluyen:
- Logo oficial de UTCBIS
- Grid de 18 logos de videojuegos participantes
- Folios incrementales automáticos
- Sección recortable para el usuario con folio destacado
- Diseño optimizado para impresión en formato A4 y Carta

## ✨ Características

- **Generación masiva**: Genera cientos de boletas automáticamente
- **Folios dinámicos**: Define un rango de folios (inicial y final) y el sistema genera todas las boletas necesarias
- **3 boletas por página**: Optimizado para impresión eficiente
- **Formato PDF**: Descarga directa en formato PDF listo para imprimir
- **Compatibilidad de impresión**: Funciona correctamente en tamaño A4 y Carta
- **Interfaz intuitiva**: Fácil de usar con indicador de progreso
- **Optimizado para rendimiento**: Generación rápida incluso con cientos de boletas

## 🚀 Instalación

1. Clona el repositorio:
```bash
git clone https://github.com/AndresGarciaLv/generador-boletas-rifa.git
cd generador-boletas-rifa
```

2. Instala las dependencias:
```bash
npm install
```

## 💻 Uso

1. Inicia el servidor de desarrollo:
```bash
npm run dev
```

2. Abre tu navegador en `http://localhost:3000`

3. Configura los folios:
   - **Folio inicial**: El número del primer folio (ej: 1, 10, 100)
   - **Folio final**: El número del último folio (ej: 300, 500, 1000)
   - El sistema calculará automáticamente cuántas páginas se necesitan (3 boletas por página)

4. Haz clic en "Descargar PDF" para generar y descargar el archivo

5. El archivo se descargará con el nombre: `boletas-rifa-folio-[inicial]-[final].pdf`

## 📁 Estructura del Proyecto

```
generador-boletas-rifa/
├── logos-stands/          # Logos de los videojuegos participantes
│   ├── utcbis-logo.png    # Logo oficial de UTCBIS
│   └── [logos de videojuegos]
├── src/
│   ├── components/
│   │   └── BoletaRifa.vue # Componente de boleta individual
│   ├── App.vue            # Componente principal con controles
│   ├── main.js            # Punto de entrada de la aplicación
│   └── style.css          # Estilos globales
├── index.html             # HTML principal
├── package.json           # Dependencias del proyecto
├── vite.config.js         # Configuración de Vite
└── README.md              # Este archivo
```

## 🎮 Videojuegos Incluidos

Las boletas incluyen logos de los siguientes videojuegos:
- Nestelia
- Kyrie Eleison - El Convento
- Ice Cube Skull
- Purple Crystal (Dreamshards)
- Etaria
- Umbral - La Forja de las Almas
- Zombies Last Dawn
- Zoon
- Hostis Aeternus
- La Isla de Kapi
- Antes del Sol y La Luna
- The Last Farm
- Vampyr
- Vivir o Morir
- Paw Print 2110
- Anestesia
- Banana Hellfire
- Ex Tenebris

## 🛠️ Tecnologías Utilizadas

- **Vue.js 3**: Framework JavaScript progresivo
- **Vite**: Herramienta de construcción rápida
- **jsPDF**: Generación de archivos PDF
- **html2canvas**: Conversión de HTML a imagen para el PDF

## 📝 Scripts Disponibles

- `npm run dev`: Inicia el servidor de desarrollo
- `npm run build`: Construye la aplicación para producción
- `npm run preview`: Previsualiza la construcción de producción

## 📄 Licencia

Este proyecto fue desarrollado para el evento de videojuegos de la Universidad Tecnológica de Cancún (UT Cancún).

## 👨‍💻 Autor

Desarrollado para el evento de videojuegos UT Cancún

---

**Nota**: Asegúrate de tener todos los logos en la carpeta `logos-stands/` para que las boletas se generen correctamente.
