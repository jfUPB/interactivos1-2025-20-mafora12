
# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](https://editor.p5js.org/mafora12/sketches/cs0ej26yO)

Código a modificar:

``` js
// P_2_1_3_01
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * changing circle amount, size and position in a grid
 *
 * MOUSE
 * position x          : circle amount and size
 * position y          : circle position
 * left click          : random position
 *
 * KEYS
 * s                   : save png
 */
'use strict';

var count = 0;
var tileCountX = 10;
var tileCountY = 10;
var tileWidth = 0;
var tileHeight = 0;

var colorStep = 15;

var circleCount = 0;
var endSize = 0;
var endOffset = 0;

var actRandomSeed = 0;

function setup() {
  createCanvas(800, 800);
  tileWidth = width / tileCountX;
  tileHeight = height / tileCountY;
  noFill();
  stroke(0, 128);
}

function draw() {
  background(255);
  randomSeed(actRandomSeed);

  translate(tileWidth / 2, tileHeight / 2);

  circleCount = mouseX / 30 + 1;
  endSize = map(mouseX, 0, max(width, mouseX), tileWidth / 2, 0);
  endOffset = map(mouseY, 0, max(height, mouseY), 0, (tileWidth - endSize) / 2);

  for (var gridY = 0; gridY <= tileCountY; gridY++) {
    for (var gridX = 0; gridX <= tileCountX; gridX++) {
      push();
      translate(tileWidth * gridX, tileHeight * gridY);
      scale(1, tileHeight / tileWidth);

      var toggle = int(random(0, 4));
      if (toggle == 0) rotate(-HALF_PI);
      if (toggle == 1) rotate(0);
      if (toggle == 2) rotate(HALF_PI);
      if (toggle == 3) rotate(PI);

      // draw module
      for (var i = 0; i < circleCount; i++) {
        var diameter = map(i, 0, circleCount, tileWidth, endSize);
        var offset = map(i, 0, circleCount, 0, endOffset);
        ellipse(offset, 0, diameter, diameter);
      }
      pop();
    }
  }
}

function mousePressed() {
  actRandomSeed = random(100000);
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}
```

[Enlace a la aplicación modificada](https://editor.p5js.org/mafora12/sketches/i-LAvP_Pc)

Código modificado:

``` js
/**
 * changing circle amount, size and position in a grid
 * * MICRO:BIT
 * acelerómetro x    : circle amount and size
 * acelerómetro y    : circle position
 * botón A           : random position
 * botón B           : save png
 */

'use strict';

let tileCountX = 10;
let tileCountY = 10;
let tileWidth = 0;
let tileHeight = 0;

let circleCount = 0;
let endSize = 0;
let endOffset = 0;

let actRandomSeed = 0;

// ----- micro:bit -----
let port;
let connectBtn;
let microBitX = 0;
let microBitY = 0;
let buttonA = false;
let buttonB = false;

let prevButtonA = false;
let prevButtonB = false;

let state = "WAIT_MICROBIT_CONNECTION";

function setup() {
  createCanvas(800, 800);
  tileWidth = width / tileCountX;
  tileHeight = height / tileCountY;
  noFill();
  stroke(0, 128);

  // Conexión con micro:bit
  port = createSerial();
  connectBtn = createButton("Conectar micro:bit");
  connectBtn.mousePressed(connectToMicrobit);
  
  // Mueve el botón de conexión a la posición deseada
  connectBtn.position(width / 2 - connectBtn.width / 2, height / 2 + 50);
}

function draw() {
  if (state === "WAIT_MICROBIT_CONNECTION") {
    background(220);
    textAlign(CENTER, CENTER);
    textSize(24);
    text("Conecta el micro:bit para comenzar", width/2, height/2);
    connectBtn.show();
    return;
  }
  
  // Oculta el botón cuando está conectado
  connectBtn.hide();

  // Lee los datos del puerto serial solo si está abierto y hay datos disponibles
  if (port.opened() && port.availableBytes() > 0) {
    serialEvent();
  }

  // Estado RUNNING
  background(255);
  randomSeed(actRandomSeed);

  translate(tileWidth / 2, tileHeight / 2);

  // usar acelerómetro
  circleCount = int(map(microBitX, -1024, 1024, 1, 20));
  endSize = map(microBitX, -1024, 1024, tileWidth / 2, 0);
  endOffset = map(microBitY, -1024, 1024, 0, (tileWidth - endSize) / 2);

  for (let gridY = 0; gridY < tileCountY; gridY++) {
    for (let gridX = 0; gridX < tileCountX; gridX++) {
      push();
      translate(tileWidth * gridX, tileHeight * gridY);
      scale(1, tileHeight / tileWidth);

      let toggle = int(random(0, 4));
      if (toggle == 0) rotate(-HALF_PI);
      if (toggle == 1) rotate(0);
      if (toggle == 2) rotate(HALF_PI);
      if (toggle == 3) rotate(PI);

      // draw module
      for (let i = 0; i < circleCount; i++) {
        let diameter = map(i, 0, circleCount, tileWidth, endSize);
        let offset = map(i, 0, circleCount, 0, endOffset);
        ellipse(offset, 0, diameter, diameter);
      }
      pop();
    }
  }

  // detectar cambios de botones
  if (buttonA && !prevButtonA) {
    actRandomSeed = random(100000);
  }
  if (buttonB && !prevButtonB) {
    saveCanvas("microbit_grid", "png");
  }

  prevButtonA = buttonA;
  prevButtonB = buttonB;
}

function connectToMicrobit() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    // El estado se cambia a RUNNING cuando la conexión se abre correctamente
    // no usamos port.on('open') porque no está disponible la versión
    state = "RUNNING";
  } else {
    port.close();
    state = "WAIT_MICROBIT_CONNECTION";
    connectBtn.html("Conectar micro:bit");
  }
}

function serialEvent() {
  let line = port.readUntil("\n");
  if (line.length > 0) {
    let values = split(trim(line), ",");
    if (values.length === 4) {
      microBitX = int(values[0]);
      microBitY = int(values[1]);
      buttonA = values[2] === "True";
      buttonB = values[3] === "True";
    }
  }
}
```

## Video

[Video demostratativo 1](https://youtu.be/-YNoua4A0fU)  
[Video demostratativo 2](https://youtu.be/0SpqrBOHnf8)








