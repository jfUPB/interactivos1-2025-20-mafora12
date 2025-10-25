# Evidencias de la unidad 8  
## Actividad 1  
### Referentes visuales
<img width="1080" height="1920" alt="image" src="https://github.com/user-attachments/assets/92055006-eaca-434b-bdf9-3603bd26999a" />  
<img width="1200" height="1024" alt="image" src="https://github.com/user-attachments/assets/4b9fd73b-c591-42f9-8896-d7d5fb84f259" />  
<img width="1152" height="2048" alt="image" src="https://github.com/user-attachments/assets/68013773-28ab-4f7b-b725-45be8f52d00e" />

Los referentes visuale son enfocados en la combinación estre lo etereo y el universo, se crean pequeños circulos como notas en un piano que viajan al universo y explotan. Me base en la estetica de los cuadros que sugieren movimiento. Ademas toma un poco el funcionamiento de piano tails para el movimiento en el telefono y el micro:bit. 

### Concepto visual  
Color Jam representa una explosión de color y sonido, donde cada nota y movimiento genera una reacción visual distinta, como si las teclas del piano liberaran fragmentos de galaxia. El objetivo es sincronizar música y movimiento humano en un mismo espacio interactivo.  
- Cuando suena el piano: las formas se expanden según la intensidad del sonido.  
- Cuando el usuario se mueve o toca la pantalla: las partículas cambian de dirección.  
- El resultado: una experiencia visual que mezcla arte sonoro, color y energía física en tiempo real.

### ¿cómo el móvil y el micro:bit controlarán las visuales?   

#### Móvil  
Detecta la posición del toque y la rotación, enviando colores y movimientos que crean partículas dinámicas en pantalla.  
#### micro:bit   
Sus botones alternan el color de los círculos y del fondo, y al agitarlo genera destellos animados.  

### Bocetos    
![Imagen de WhatsApp 2025-10-24 a las 17 34 32_7964380c](https://github.com/user-attachments/assets/19067fbc-7858-4676-a5de-b74062ade061)  
![Imagen de WhatsApp 2025-10-24 a las 17 34 33_5f305cb3](https://github.com/user-attachments/assets/2375c886-d497-4c7f-948d-9d5f2518ea74)  

### Diagrama  

<img width="917" height="1118" alt="image" src="https://github.com/user-attachments/assets/deb9cd5c-2887-44ec-9c0b-94e7804cccaa" />


## Actividad 2  
### Documentación del proceso  
Al tener la base del ejercico, decidi mejorar algunos aspectos como elfondo y el color de los circulos para que no se viera tan aburrido 
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
y mejorar los colores de los circulos para que resalten y tengan una mejor visual
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
Teniendo eso listo empece con la union con el micro:bit, la cual modifique ciertos aspectos en el scketch.js del dekstop, así:  
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
Este codigo me permitio las visuales que me generaria el micro:bit los cuales son:   
  - Boton A: cambia de color el circulo generado por el celular
  - Boton B: cambia el color de fondo
  - sheke: Agrega destellos azules a la estetica de las visuales.

Luego configure el server.js para notificar los mensajes del celular, microbit y los clientes:   
```python
