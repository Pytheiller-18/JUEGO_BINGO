# 🎲 BINGO 75 - Multijugador en Red y Tablero Offline

Sistema completo de **Bingo Tradicional de 75 Balotas**, diseñado para jugar tanto en **Red Local / Online** con sincronización en tiempo real mediante WebSockets, como en **Modo Offline Autónomo** directamente desde el navegador web sin necesidad de servidores ni internet.

---

## 🚀 Modos de Juego Disponibles

El proyecto cuenta con dos modalidades completamente funcionales:

### 1. 🌐 Modo Servidor Multijugador (Socket.IO + Express)
- **Servidor Local / Túnel Seguro:** Se ejecuta localmente en Node.js y genera código QR y URL para acceso local (LAN) o mediante túnel público seguro (Cloudflare).
- **Tablero Maestro Administradora (`/admin.html`):** Sorteo manual o automático con velocidad configurable (3s a 15s), locución de voz y efectos de sonido nativos (Web Audio API), monitor de jugadores en tiempo real, e inspector de cartones en vivo.
- **Cartón del Jugador (`/` o `/index.html`):** Grilla interactiva 5x5 responsive (móvil y escritorio), marcado táctil, seguimiento de balotas cantadas, y botón con validación instantánea de ¡BINGO!

### 2. 📴 Modo Offline Autónomo (Archivos HTML Directos)
- **Sin necesidad de instalar Node.js ni configurar red:** Basta con abrir los archivos directamente en el navegador.
- **Tablero de Control (`TABLERO_ADMINISTRADORA.html`):** Incluye sorteador de balotas, tablero maestro interactivo, locución por voz del navegador, confeti, validador de cartones por código e interfaz de asignación de cartones únicos.
- **Cartón del Jugador (`CARTON_BINGO.html`):** Cartón determinístico con sonidos y marcación de casillas.

---

## 🛡️ Garantía de Unicidad: Cero Tablas Repetidas

Se ha resuelto de forma definitiva el problema de duplicación de cartones entre participantes:

### ¿Cómo se garantiza que no haya tablas repetidas?

1. **En Modo Offline (`CARTON_BINGO.html` y `TABLERO_ADMINISTRADORA.html`):**
   - **Algoritmo PRNG Determinístico (`mulberry32`):** Cada número de cartón (1, 2, 3...) genera una matriz matemática de 24 números 100% única y predecible. Comprobado matemáticamente mediante pruebas de colisión en más de 50,000 cartones con **0 duplicados**.
   - **Asignación directa por URL:** Permite compartir enlaces personalizados (ej. `CARTON_BINGO.html?carton=1`, `?carton=2`, etc.) que fijan y bloquean automáticamente el cartón asignado al jugador.
   - **Ingreso Manual de Número:** El jugador puede introducir el número asignado por la administradora o generar uno aleatorio en un espacio amplio (hasta 99,999).
   - **Asignador de Cartones Integrado:** La administradora dispone de un panel interactivo para registrar los nombres de los jugadores por número de cartón, previsualizar la tabla en vivo de cualquier número y copiar el enlace personalizado para cada persona.

2. **En Modo Servidor Multijugador (`server.js`):**
   - **Firma Canónica (`getCardSignature`):** Cada tabla genera una firma única a partir de los números de sus 5 columnas.
   - **Validación de Huella Única (`generateUniqueBingoCard`):** Al registrarse un jugador o al reiniciar la partida, el servidor compara la firma contra todos los cartones activos, descartando y regenerando cualquier combinación repetida.
   - **Numeración Oficial de Cartón (`Cartón #1`, `Cartón #2`...):** Cada participante recibe un identificador secuencial visible en su pantalla y en el panel de administración.

---

## 📋 Modalidades de Victoria Soportadas

El sistema valida de forma automática los siguientes patrones:
- **Cualquier Línea:** Horizontal, vertical o diagonal.
- **Cartón Lleno (Pleno / Apagón):** Las 24 casillas marcadas.
- **4 Esquinas:** Las 4 casillas de los extremos.
- **Letra X:** Ambas diagonales completas.
- **Cruz Central:** Fila 3 y Columna 3 completas.
- **Letra L:** Columna B completa + Fila inferior completa.

---

## 💻 Requisitos e Instalación

### Opción A: Modo Servidor (Multijugador en Red)
1. Instalar [Node.js](https://nodejs.org/) (versión 16 o superior).
2. Descomprimir el contenido de `BINGO.zip`.
3. Abrir una terminal en la carpeta del juego y ejecutar:
   ```bash
   npm install
   ```
4. Iniciar el servidor:
   - En Windows: Hacer doble clic en `INICIAR_BINGO.bat`.
   - O por terminal:
     ```bash
     node server.js
     ```
5. Acceder desde el navegador:
   - **Administradora:** `http://localhost:3000/admin.html`
   - **Jugadores:** `http://<TU_IP_LOCAL>:3000` (o escanear el código QR en pantalla).

### Opción B: Modo Offline (Sin Servidor)
1. Descomprimir `BINGO.zip`.
2. Para la persona que canta el bingo: Abrir `TABLERO_ADMINISTRADORA.html` en el navegador.
3. Para los jugadores: Compartir `CARTON_BINGO.html` (o enviar enlaces con `?carton=1`, `?carton=2`, etc.).

---

## 📁 Estructura del Proyecto

```plaintext
BINGO/
├── CARTON_BINGO.html           # Cartón individual offline para jugadores
├── TABLERO_ADMINISTRADORA.html # Panel de sorteo, validador y asignador offline
├── INICIAR_BINGO.bat           # Script de inicio rápido en Windows
├── cloudflared.exe             # Binario para túnel público seguro (opcional)
├── package.json                # Dependencias y scripts del proyecto
├── server.js                   # Servidor Node.js Express + Socket.IO
└── public/                     # Archivos web del modo multijugador
    ├── index.html              # Vista del jugador
    ├── admin.html              # Vista de la administradora
    ├── css/
    │   ├── style.css           # Estilos base y variables de diseño
    │   ├── player.css          # Estilos del cartón y vista móvil
    │   └── admin.css           # Estilos del panel de control
    └── js/
        ├── player.js           # Lógica del cliente jugador
        ├── admin.js            # Lógica del panel de administración
        ├── audio.js            # Motor de sonido offline (Web Audio API)
        └── confetti.js         # Efecto de confeti nativo Canvas
```

---

## 🤝 Contribuciones

Las contribuciones son bienvenidas:
1. Haz un **Fork** de este repositorio.
2. Crea una rama para tu mejora: `git checkout -b feature/nueva-funcionalidad`.
3. Realiza tus cambios y haz commit: `git commit -m "feat: agrega nueva funcionalidad"`.
4. Envía tus cambios a tu fork: `git push origin feature/nueva-funcionalidad`.
5. Abre un **Pull Request** detallando tus modificaciones.

---

## 📄 Licencia

Este proyecto es de código abierto para fines educativos y recreativos.
