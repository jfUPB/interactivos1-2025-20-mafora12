# Evidencias de la unidad 8  
## Actividad 1
<a name="actividad1"></a>
### Referentes visuales
<a name="p1"></a>
<img width="1080" height="1920" alt="image" src="https://github.com/user-attachments/assets/92055006-eaca-434b-bdf9-3603bd26999a" />  
<img width="1200" height="1024" alt="image" src="https://github.com/user-attachments/assets/4b9fd73b-c591-42f9-8896-d7d5fb84f259" />  
<img width="1152" height="2048" alt="image" src="https://github.com/user-attachments/assets/68013773-28ab-4f7b-b725-45be8f52d00e" />

Los referentes visuale son enfocados en la combinación estre lo etereo y el universo, se crean pequeños circulos como notas en un piano que viajan al universo y explotan. Me base en la estetica de los cuadros que sugieren movimiento. Ademas toma un poco el funcionamiento de piano tails para el movimiento en el telefono y el micro:bit. 

### Concepto visual  
<a name="p2"></a>
Color Jam representa una explosión de color y sonido, donde cada nota y movimiento genera una reacción visual distinta, como si las teclas del piano liberaran fragmentos de galaxia. El objetivo es sincronizar música y movimiento humano en un mismo espacio interactivo.  
- Cuando suena el piano: las formas se expanden según la intensidad del sonido.  
- Cuando el usuario se mueve o toca la pantalla: las partículas cambian de dirección.  
- El resultado: una experiencia visual que mezcla arte sonoro, color y energía física en tiempo real.

### ¿cómo el móvil y el micro:bit controlarán las visuales?   
<a name="p3"></a>
#### Móvil  
Detecta la posición del toque y la rotación, enviando colores y movimientos que crean partículas dinámicas en pantalla.  
#### micro:bit   
Sus botones alternan el color de los círculos y del fondo, y al agitarlo genera destellos animados.  

### Bocetos    
<a name="p4"></a>
![Imagen de WhatsApp 2025-10-24 a las 17 34 32_7964380c](https://github.com/user-attachments/assets/19067fbc-7858-4676-a5de-b74062ade061)  
![Imagen de WhatsApp 2025-10-24 a las 17 34 33_5f305cb3](https://github.com/user-attachments/assets/2375c886-d497-4c7f-948d-9d5f2518ea74)  

### Diagrama  
<a name="p5"></a>
<img width="917" height="1118" alt="image" src="https://github.com/user-attachments/assets/deb9cd5c-2887-44ec-9c0b-94e7804cccaa" />


## Actividad 2   
<a name="actividad2"></a>
### Documentación del proceso   
<a name="p6"></a>
Al tener la base del ejercicio original, decidí mejorar algunos aspectos visuales para que no se viera tan plano o aburrido.
Mi objetivo era darle más profundidad y dinamismo al fondo, además de hacer que los círculos resaltaran más con mejores colores y degradados.  
Primero, modifiqué el fondo para que tuviera un degradado dinámico, que aportara más vida al escenario:  
```python
function drawBackground() {
  // --- Degradado dinámico ---
  for (let y = 0; y < height; y++) {
    let inter = map(y, 0, height, 0, 1);
    let c = lerpColor(color(210, 70, 25), color(280, 60, 25), inter);
    stroke(c);
    line(0, y, width, y);
  }  
```   
Luego, mejoré los colores de los círculos reactivos para que resaltaran mejor y se integraran de forma estética con el fondo:  
```python
function drawReactiveCircle(x, y, size, alpha, level) {
  let bright = map(level, 0, 0.3, 60, 100);
  let g = drawingContext.createRadialGradient(x, y, size * 0.3, x, y, size / 2);
  g.addColorStop(0, `${circleColor}`);
  g.addColorStop(0.7, `hsla(50,100%,${bright}%,${alpha / 300})`);
  g.addColorStop(1, "rgba(255,255,255,0)");
  drawingContext.fillStyle = g;
  ellipse(x, y, size);
}
```
Estas mejoras fueron clave para lograr un aspecto más interactivo y atractivo antes de empezar con la integración del micro:bit.    

#### Integración con el micro:bit
Una vez tuve lista la parte visual, pasé a conectar el micro:bit para controlar ciertos aspectos de las visuales.
Para esto, modifiqué el archivo sketch.js del entorno desktop, añadiendo funciones que respondieran a los botones A y B, y al gesto de “shake”.  

```python
let socket;
let trails = [];
let song, fft, amplitude;
let reactiveGlow = 0;
let circleColor = "hsl(0, 0%, 100%)";
let magentaMode = false;
let grayMode = false; // 🩶 modo gris activado con botón B

let phoneData = { x: 0, y: 0, color: "hsl(0,100%,100%)", touch: false };
let microData = { accel: 0, buttonA: false, buttonB: false, shake: false };

let bgGraphics;
let starPositions = [];
let cyanBursts = []; // 💥 destellos animados

function preload() {
  song = loadSound("/Camille Saint-Saëns - Danse Macabre.mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB);
  noStroke();

  bgGraphics = createGraphics(width, height);
  drawBackground(bgGraphics);

  socket = io();
  socket.on("connect", () => console.log("🖥️ Conectado al servidor"));

  socket.on("mobileData", (data) => {
    phoneData = data;
    trails.push({
      x: data.x,
      y: data.y,
      color: circleColor,
      life: 255,
    });
    if (trails.length > 50) trails.shift();
  });

  socket.on("microbitData", (data) => {
    // Botón A → alterna color del círculo
    if (data.buttonA && !microData.buttonA) {
      magentaMode = !magentaMode;
      circleColor = magentaMode
        ? "hsl(300, 100%, 70%)"
        : "hsl(0, 0%, 100%)";
      console.log(magentaMode ? "🌸 Modo magenta activado" : "⚪ Modo blanco restaurado");
    }

    // Botón B → alterna fondo entre color y gris
    if (data.buttonB && !microData.buttonB) {
      grayMode = !grayMode;
      if (grayMode) {
        drawGrayBackground(bgGraphics);
        console.log("🌫️ Fondo gris activado");
      } else {
        drawBackground(bgGraphics);
        console.log("🌌 Fondo original restaurado");
      }
    }

    // Shake → genera destellos animados
    if (data.shake) {
      console.log("💥 Agitado!");
      createCyanBursts();
    }

    microData = data;
  });

  fft = new p5.FFT(0.8, 64);
  amplitude = new p5.Amplitude();
}
```
Con este código, el micro:bit controla las visuales de esta forma:  
- Botón A: cambia el color del círculo generado por el móvil.  
- Botón B: alterna entre un fondo colorido y uno gris.  
- Shake: genera destellos azules animados sobre las visuales.

#### Configuración del servidor  
Para conectar el micro:bit y el móvil al mismo tiempo, modifiqué el archivo server.js para que emitiera y recibiera correctamente los mensajes de ambos.  

```python
// 🌈 COLOR JAM SERVER - Integración total con micro:bit
const express = require("express");
const http = require("http");
const socketIO = require("socket.io");
const { SerialPort } = require("serialport");
const { ReadlineParser } = require("@serialport/parser-readline");

const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;

// Sirve la carpeta public (donde está tu index.html y sketch.js)
app.use(express.static("public"));

// 💻 WebSocket conexión general
io.on("connection", (socket) => {
  console.log("🟢 Nuevo cliente conectado");

  // 🎨 Datos del móvil
  socket.on("mobileData", (data) => {
    io.emit("mobileData", data);
  });

  // 🧠 Datos del micro:bit enviados manualmente (opcional)
  socket.on("microbitData", (data) => {
    io.emit("microbitData", data);
  });

  socket.on("disconnect", () => {
    console.log("🔴 Cliente desconectado");
  });
});

// 🧩 MICRO:BIT vía Puerto Serial (ajusta el COM según tu PC)
const portPath = "COM13"; //  cambia este número si tu micro:bit usa otro puerto
const serial = new SerialPort({ path: portPath, baudRate: 115200 });
const parser = serial.pipe(new ReadlineParser({ delimiter: "\n" }));

parser.on("data", (line) => {
  try {
    const data = JSON.parse(line);
    io.emit("microbitData", data); // Enviar a todos los clientes conectados
    console.log("📤 microbitData:", data);
  } catch (err) {
    console.log("⚠️ Error leyendo micro:bit:", err);
  }
});

server.listen(port, () => {
  console.log(`🚀 Servidor en: http://localhost:${port}`);
  console.log("🔌 Esperando datos del micro:bit en", portPath);
});
```
#### Problemas con el micro:bit y soluciones  

Al principio, el micro:bit no enviaba los datos correctamente.
Estaba usando el editor MicroPython online, y aunque parecía funcionar, el formato JSON no era totalmente compatible.
Usaba este código:
```python
from microbit import *
import json

uart.init(baudrate=115200)

while True:
    data = {
        "buttonA": button_a.is_pressed(),
        "buttonB": button_b.is_pressed(),
        "shake": accelerometer.was_gesture("shake")
    }
    uart.write(json.dumps(data) + "\n")
    sleep(200)
```  
Sin embargo, este formato generaba errores, así que cambié la estructura a texto plano estilo JSON, lo cual sí funcionó correctamente:  
```python
from microbit import *

uart.init(baudrate=115200)

while True:
    # Creamos el texto como si fuera JSON
    data = '{ "buttonA": %s, "buttonB": %s, "shake": %s }\n' % (
        "true" if button_a.is_pressed() else "false",
        "true" if button_b.is_pressed() else "false",
        "true" if accelerometer.was_gesture("shake") else "false"
    )
    
    uart.write(data)
    sleep(200)
```
Finalmente, este código fue el que logró enviar los datos correctamente al servidor.  

#### Error final y solución en el index.html   
Fue entonces cuando le pregunté a ChatGPT si mi index.html del desktop tenía algo mal.
Ahí descubrí que necesitaba un botón de conexión manual al puerto serial, para que el navegador pidiera permiso de acceso al micro:bit.  

- Prompt:  
  " este es mi index.html del dekstop
  ```python
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="utf-8" />
  <title>ColorJam - Visualizador</title>
  <script src="/socket.io/socket.io.js"></script>
  <!-- Solo una librería de p5.js -->
  <script src="libraries/p5.min.js"></script>
  <!-- p5.sound debe ir después de p5 -->
  <script src="libraries/p5.sound.min.js"></script>
  <script src="sketch.js"></script>
  <style>
   body {
  margin: 0;
  overflow: hidden;
  background: black;
  }
  </style>
  </head>
  <body></body>
  </html>"
  ```
  - Respuesta con la version mejorada  
    ```python
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8" />
    <title>ColorJam - Visualizador</title>
    <script src="/socket.io/socket.io.js"></script>
    <!-- Solo una librería de p5.js -->
    <script src="libraries/p5.min.js"></script>
    <!-- p5.sound debe ir después de p5 -->
    <script src="libraries/p5.sound.min.js"></script>
    <script src="sketch.js"></script>
    <style>
      body {
        margin: 0;
        overflow: hidden;
        background: black;
      }

      /* Botón flotante arriba a la izquierda */
      #connectBtn {
        position: fixed;
        top: 10px;
        left: 10px;
        background: #ff0044;
        border: none;
        border-radius: 8px;
        color: white;
        padding: 10px 16px;
        font-size: 14px;
        cursor: pointer;
        z-index: 1000;
      }

      #connectBtn:hover {
        background: #ff3366;
      }
    </style>
    </head>

    <body>
    <button id="connectBtn">🔌 Conectar micro:bit</button>

    <script>
      // Variables globales accesibles desde sketch.js
      window.microData = { buttonA: false, buttonB: false, shake: false };

      document.getElementById("connectBtn").addEventListener("click", async () => {
        try {
          const port = await navigator.serial.requestPort();
          await port.open({ baudRate: 115200 });
          const reader = port.readable.getReader();
          console.log("✅ micro:bit conectado");
          let buffer = "";

          while (true) {
            const { value, done } = await reader.read();
            if (done) break;
            buffer += new TextDecoder().decode(value);

            // Procesar cada línea JSON enviada por la micro:bit
            const lines = buffer.split("\n");
            buffer = lines.pop();

            for (let line of lines) {
              try {
                const data = JSON.parse(line.trim());
                window.microData = data; // 👈 Esto actualiza los datos globales
              } catch (e) {
                // ignora líneas vacías o corruptas
              }
            }
          }
        } catch (err) {
          console.error("❌ Error conectando micro:bit:", err);
          alert("Error conectando al micro:bit. Usa Chrome o Edge y acepta permisos.");
        }
      });
    </script>
    </body>
    </html>
    ```  
Con este último cambio, todo funcionó perfectamente. Los datos del micro:bit comenzaron a recibirse correctamente, y finalmente logré la integración total con las visuales de Color Jam.  

### Codigos  
<a name="p7"></a>
#### Micro:bit  
  ```python
  from microbit import *

  uart.init(baudrate=115200)

  while True:
    # Creamos el texto como si fuera JSON
    data = '{ "buttonA": %s, "buttonB": %s, "shake": %s }\n' % (
        "true" if button_a.is_pressed() else "false",
        "true" if button_b.is_pressed() else "false",
        "true" if accelerometer.was_gesture("shake") else "false"
    )
    
    uart.write(data)
    sleep(200)
   ```

#### Proyecto general  

  Link del proyecto: https://github.com/mafora12/actividad_5.git
  
## Autoevalución  

| Actividades | Nota | justificación |
| --- | --- | --- |
| 1 | 5.0 | Se evidencia la [actividad 1](#actividad1) completa, con las 5  ([p1](#p1), [p2](#p2), [p3](#p3), [p4](#p4),[p5](#p5) ) preguntas completadas y su respectiva verificación |
| 2 | 5.0 | Se evidencia la [actividad  2](#actividad2)completa, con la documentación y wl link de todos los dodigos ([p6](#p6), [p7](#p7)) |
| nota final del promedio | 5.0 |  |




