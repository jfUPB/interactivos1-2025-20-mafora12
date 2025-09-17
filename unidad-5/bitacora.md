# Evidencias de la unidad 5

## Actividad 2

### ¿Qué datos envía el micro:bit?  
El micro:bit se comunica con el sketch en p5.js a través del puerto serie (UART) usando un protocolo ASCII. Cada 100 ms, el micro:bit toma lecturas del acelerómetro en los ejes X y Y, además del estado de los botones A y B, y construye una cadena con esos valores separada por comas y terminada en un salto de línea:  

``` js
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
uart.write(data)
```
Usar ASCII hace que sea más fácil leer lo que manda el micro:bit y revisar en un monitor serial qué datos se están transmitiendo. La desventaja es que gasta más espacio, porque en vez de mandar los números en binario, los envía como texto caracter por caracter.

### ¿Cómo es la estructura del protocolo ASCII usado?
El protocolo sigue una estructura simple:  
``` js
xValue, yValue, aState, bState\n
```

"xValue, yValue" -> enteros del acelerómetro.  

"aState, bState"->booleanos (True o False).  

"," → delimitador que permite dividir los datos.  

"\n" → marca el final de la transmisión.   

Este método no es tan seguro ni tan eficiente, porque si se llega a dañar o perder la coma que separa los datos, todo el mensaje queda malo. En cambio, un protocolo binario sería más rápido y estable, pero también más difícil de entender. En este caso se eligió que sea más claro aunque no sea lo más optimizado.

### Lectura y transformación de los datos en p5.js  

La función draw() se encarga de verificar si hay datos disponibles en el puerto serie. Luego los lee hasta encontrar el salto de línea (\n), los divide por comas y los convierte en variables numéricas o booleanas:

``` js
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
  if (data) {
    data = data.trim();
    let values = data.split(",");
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    }
  }
}

```

Se ve cómo los datos que llegan en ASCII se transforman en coordenadas de la pantalla (X, Y) y en el estado de los botones. Eso muestra que el protocolo no solo manda información, sino que también organiza el mensaje para que cada valor tenga un lugar y un sentido específico.

###  ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?   

La función "updateButtonStates()" compara los valores actuales con los anteriores para detectar transiciones:  
``` js
function updateButtonStates(newAState, newBState) {
if (newAState === true && prevmicroBitAState === false) {
  lineModuleSize = random(50, 160);
  clickPosX = microBitX;
  clickPosY = microBitY;
  print("A pressed");
}

if (newBState === false && prevmicroBitBState === true) {
  c = color(random(255), random(255), random(255), random(80, 100));
  print("B released");
}
```
Esto no funciona como un sistema que detecta cuándo un botón pasa de estar presionado a no estarlo (o al revés), parecido a los cambios de estado. Gracias a eso, los datos que se mandan en ASCII se convierten en acciones gráficas: cada vez que aprieto o suelto un botón en el micro:bit, eso se refleja en un cambio visual dentro del lienzo en p5.js.

## Dibujos
<img width="1918" height="937" alt="image" src="https://github.com/user-attachments/assets/277e9abb-df2d-40ea-a488-e8ad51ad0558" />
<img width="1918" height="932" alt="image" src="https://github.com/user-attachments/assets/c8080c4b-e7cf-491f-afc4-ff358cf58ae9" />


## Actividad 2  

### ¿Por qué se ve este resultado?
<img width="1233" height="552" alt="image" src="https://github.com/user-attachments/assets/291af6f7-14cb-4e74-90a1-51cfda9efa55" />  

Lo que se ve en la imagen con tantos símbolos raros pasa porque el micro:bit está enviando datos en binario con el struct.pack, y en la aplicación yo los estoy mostrando como texto. Como esos datos no corresponden a letras ni números, el programa intenta leerlos como si fueran texto y por eso aparecen esos caracteres sin sentido.  

<img width="1237" height="407" alt="image" src="https://github.com/user-attachments/assets/8103990e-a211-4551-9f94-ca3968b10218" />  
Aqui se empaquetan los valores del acelerómetro y los botones en formato binario (dos enteros de 16 bits y dos bytes). En la captura, cada grupo de dos caracteres en hex representa un byte de esos datos empaquetados. Sí, el resultado es mucho más difícil de leer que cuando se mandan valores en ASCII, porque en vez de ver números claros, hay que interpretar los bytes y traducirlos para saber qué significa cada uno.

 - Ventajas del formato binario
    - Ocupa menos espacio, ya que se transmiten solo los bytes necesarios.
    - Es más rápido de enviar y recibir, especialmente si hay muchos datos.
    - Reduce errores al no depender de caracteres adicionales (comas, saltos de línea).

- Desventajas del formato binario
  - Es mucho más difícil de leer e interpretar a simple vista.
  - Necesitas conocer la estructura exacta de los datos para poder decodificarlos.
  - No es práctico para depurar o revisar sin herramientas adicionales.
<img width="1263" height="577" alt="image" src="https://github.com/user-attachments/assets/d4e4902b-9afe-4518-9e40-913d285a5a69" />
- El formato '>2h2B' significa:
  
">" -> orden big-endian (los bytes más significativos van primero).

"2h" -> dos enteros de 16 bits con signo (2 bytes cada uno).  

"2B" -> dos enteros de 8 bits sin signo (1 byte cada uno).   

Entonces:  
xValue → ocupa 2 bytes.  
yValue → ocupa 2 bytes.  
aState → ocupa 1 byte. 
bState → ocupa 1 byte.  
En total, cada mensaje son 6 bytes enviados.

- ¿Qué significa cada byte?
  Los primeros 2 bytes: valor de xValue (puede ser positivo o negativo).
  Los siguientes 2 bytes: valor de yValue (también con signo).
  El quinto byte: estado del botón A (0 = no presionado, 1 = presionado).
  El sexto byte: estado del botón B (0 = no presionado, 1 = presionado).

- Como h es un entero con signo (16 bits), se codifica en complemento a dos.  
Ejemplo:
xValue = 100 → en hex: 00 64.  
xValue = -100 → en hex: FF 9C.  
Esto pasa igual con yValue.  
Los botones en cambio siempre serán 00 o 01, porque son enteros sin signo de 8 bits.

<img width="1230" height="541" alt="image" src="https://github.com/user-attachments/assets/468cc386-595e-476b-ac0d-fa3fd4d534a9" />  
Diferencias entre ASCII y binario  

Binario (HEX): los datos aparecen como bytes crudos, difíciles de leer sin saber el formato (>2h2B).  
ASCII: se muestran como números y caracteres legibles (ej: -356, 572, 0, 1).  

Binario:   

- Ventanjas  
  - Ocupa menos espacio: cada número se guarda en su tamaño real (2 o 1 byte).  
  - Transmisión más rápida y eficiente.  
  - No pierde precisión con enteros grandes o negativos.  

- Desventajas:  
  - Difícil de interpretar a simple vista.  
  - Necesita que emisor y receptor conozcan el mismo formato (endianess, tamaño de cada campo).  

Formato ASCII   

- Ventajas:    
  - Muy fácil de leer e interpretar en cualquier terminal.  
  - No necesitas conocer la estructura de datos para ver la información.  
  - Más universal: cualquier programa que lea texto puede interpretarlo.  

- Desventajas:
  
  - Consume más espacio: un número puede ocupar varios bytes solo para representarse en texto.  
  - Transmisión más lenta.  
  - Conversión entre texto y número requiere más procesamiento.  


## Actividad 3

### Explica:  
En la unidad anterior era necesario enviar los datos en formato ASCII, por lo que cada número podía ocupar una cantidad diferente de caracteres; por eso se usaban delimitadores como comas para separar los valores y un salto de línea para indicar el final del paquete. En cambio, ahora al trabajar en formato binario con struct.pack(), cada dato ocupa un tamaño fijo en bytes (2 bytes para xValue y yValue, y 1 byte para aState y bState), lo que hace que el paquete completo tenga siempre la misma longitud (6 bytes). De esta forma, el receptor puede identificar de manera exacta cada paquete sin necesidad de delimitadores ni saltos de línea.

### ¿Qué cambios observas?

La diferencia es que antes los datos se enviaban en texto ASCII, entonces había que esperar hasta un salto de línea, separar la cadena con comas y convertir cada valor, lo que hacía el proceso más largo y dependía de delimitadores. En cambio, ahora se usan paquetes binarios de tamaño fijo (6 bytes), así que basta con leerlos directamente y extraer cada dato con sus posiciones exactas, lo que hace la comunicación más rápida y sin necesidad de saltos de línea ni separadores.

### ¿Qué ves en la consola? ¿Por qué crees que se produce este error?  

En la consola se ve que al inicio los datos llegan bien, pero después aparecen valores que no tienen sentido, como números muy grandes o muy pequeños. Esto pasa porque los datos se están leyendo en binario y a veces la lectura no queda bien alineada con el inicio del paquete, entonces los bytes se interpretan mal. En el formato ASCII eso no ocurría porque el salto de línea marcaba siempre dónde terminaba cada paquete, pero en binario todo depende de leer exactamente los 6 bytes correctos, y si se pierde o se descuadra un byte, los valores salen erróneos.  

### ¿Qué veo?
Lo que veo en este código es que ahora construiste un sistema más robusto de comunicación entre el micro:bit y p5.js, se usa un buffer de bytes (serialBuffer) para acumular los datos recibidos en lugar de leerlos directamente. Cada paquete tiene un formato fijo de 8 bytes, con un header (0xAA) al inicio, seguido de los datos y un checksum al final para validar que el paquete no llegó dañado. Esto soluciona el problema de “datos locos” que viste antes, porque aunque se pierdan o se desordenen algunos bytes, el receptor puede volver a sincronizarse buscando el header correcto.  

Los datos dentro del paquete siguen siendo:   
- microBitX → 2 bytes con signo.  
- microBitY → 2 bytes con signo.  
- microBitAState → 1 byte.  
- microBitBState → 1 byte.  
- el checksum → 1 byte.  

En resumen: este código ya no solo lee valores, sino que maneja paquetes binarios con validación y usa esos datos para dibujar de forma interactiva en la pantalla, con más control y estabilidad que las versiones anteriores.

###  ¿Qué cambios tienen los programas y ¿Qué puedes observar en la consola del editor de p5.js?
El cambio más grande entre los programas es que ahora ya no estoy enviando simples datos separados por comas desde el micro\:bit, sino que armo un **paquete binario más completo**. Ese paquete empieza con un **header (0xAA)**, luego van los valores de los ejes y de los botones, y al final un **checksum** que sirve para verificar que el paquete no esté dañado. Del lado de p5.js también cambió la forma de leer: antes solo hacía un "readUntil("\n")" y separaba con comas, pero ahora voy guardando los bytes en un **buffer**, busco el header, reviso que el paquete tenga el tamaño exacto y valido con el checksum antes de usar los datos.

En la consola de p5.js lo que observo son mensajes de conexión como *“Microbit ready to draw”*, los cambios de estado de los botones como *“A pressed”* o *“B released”*, y además aparecen los valores de "microBitX" y "microBitY" que se van actualizando con el acelerómetro. Esto me confirma que los datos están llegando de forma más ordenada y confiable que antes.

## Actividad 4

### Construcción   
Tomé el código de la unidad pasada, que funcionaba con datos en formato ASCII separados por comas y terminados en salto de línea ("data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)"). Ese sistema era fácil de leer en consola, pero poco eficiente porque cada número ocupaba varios caracteres.

En el micro:bit escribí el siguiente código en MicroPython, especificado en la actividad:  
``` js
from microbit import *
import struct

uart.init(115200)
display.set_pixel(0, 0, 9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
    checksum = sum(data) % 256
    packet = b'\xAA' + data + bytes([checksum])
    uart.write(packet)
    sleep(100)
```
### Pruebas y errores 
EL primer codigo que cree fue este 
``` js
/**
 * GRID + micro:bit (datos binarios)
 * 
 * Acelerómetro x : cantidad y tamaño de círculos
 * Acelerómetro y : posición
 * Botón A        : random position
 * Botón B        : save png
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

// buffer temporal para reconstruir los paquetes
let packetBuffer = [];

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

  connectBtn.position(width / 2 - connectBtn.width / 2, height / 2 + 50);
}

function draw() {
  if (state === "WAIT_MICROBIT_CONNECTION") {
    background(220);
    textAlign(CENTER, CENTER);
    textSize(24);
    text("Conecta el micro:bit para comenzar", width / 2, height / 2);
    connectBtn.show();
    return;
  }

  connectBtn.hide();

  // procesar datos binarios
  if (port.opened() && port.availableBytes() > 0) {
    readBinaryPacket();
  }

  // Estado RUNNING
  background(255);
  randomSeed(actRandomSeed);

  translate(tileWidth / 2, tileHeight / 2);

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

      for (let i = 0; i < circleCount; i++) {
        let diameter = map(i, 0, circleCount, tileWidth, endSize);
        let offset = map(i, 0, circleCount, 0, endOffset);
        ellipse(offset, 0, diameter, diameter);
      }
      pop();
    }
  }

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
    state = "RUNNING";
  } else {
    port.close();
    state = "WAIT_MICROBIT_CONNECTION";
    connectBtn.html("Conectar micro:bit");
  }
}

function readBinaryPacket() {
  while (port.availableBytes() > 0) {
    let byte = port.readByte();

    if (byte === -1) return; // no data

    packetBuffer.push(byte);

    // buscar encabezado
    if (packetBuffer[0] !== 0xAA) {
      packetBuffer.shift();
      continue;
    }

    // esperar paquete completo
    if (packetBuffer.length >= 8) {
      let packet = packetBuffer.slice(0, 8);
      packetBuffer = [];

      // verificar checksum
      let dataBytes = packet.slice(0, 7);
      let receivedChecksum = packet[7];
      let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;

      if (receivedChecksum !== computedChecksum) {
        console.warn("Checksum incorrecto");
        return;
      }

      // reconstruir valores
      let xHigh = dataBytes[0];
      let xLow = dataBytes[1];
      let yHigh = dataBytes[2];
      let yLow = dataBytes[3];

      // big-endian porque usamos '>2h2B'
      let rawX = (xHigh << 8) | (xLow & 0xFF);
      let rawY = (yHigh << 8) | (yLow & 0xFF);

      // convertir a signed 16-bit
      if (rawX & 0x8000) rawX = rawX - 0x10000;
      if (rawY & 0x8000) rawY = rawY - 0x10000;

      microBitX = rawX;
      microBitY = rawY;
      buttonA = dataBytes[4] === 1;
      buttonB = dataBytes[5] === 1;
    }
  }
}
```
y me salieron demasiados cheksum erro, y no se movia nada, porque inclui el byte de encabezado 0xAA. Eso nunca iba a coincidir con el enviado por el micro:bit.
y corregi solo una linea, y todo me dio mientas se movia 

``` js
      let dataBytes = packet.slice(1, 7);
```
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/303013eb-2bae-448f-9b4d-549d8523d3e9" />


Primero quise observar que todo este en su lugar y nada genere error, tanto en el eje X, como en el eje Y, que funcione el boton A y el boton B
observe de manera lenta que los círculos en pantalla aumentaba o disminuía progresivamente.

Luego:
Moví el micro:bit rápidamente de izquierda a derecha. círculos fluctuaban fuerte pero el programa no se congelaba.  

Cada vez que presionaba el botón A, la semilla aleatoria cambiaba y los patrones de los círculos se reorganizaban en toda la cuadrícula. Revisé en consola y se confirmaba que buttonA = 1 llegaba bien en el paquete. Luego, cuando apretaba el botón B, automáticamente se guardaba el canvas como imagen.

### Aprendizaje 
Particularmente tuve problemas con el cheksum y pude aprender un poco mas de eso:

El checksum es como una forma de revisar que los datos que manda el micro:bit lleguen completos y sin errores. Básicamente, el micro:bit suma los valores del paquete y manda ese resultado, y en mi código yo vuelvo a hacer la suma para ver si coincide. Si no coincide, sale el mensaje de “checksum incorrecto”.

Al principio me asusté porque pensé que eso significaba que el programa estaba dañado, pero en realidad lo que hace es avisar que llegó basura o que el paquete no se leyó bien. O sea, no es que todo esté mal, sino que el checksum me protege de usar datos malos.  

Siendo este el codigo final:
``` js
/**
 * GRID + micro:bit (datos binarios)
 * 
 * Acelerómetro x : cantidad y tamaño de círculos
 * Acelerómetro y : posición
 * Botón A        : random position
 * Botón B        : save png
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

// buffer temporal para reconstruir los paquetes
let packetBuffer = [];

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

  connectBtn.position(width / 2 - connectBtn.width / 2, height / 2 + 50);
}

function draw() {
  if (state === "WAIT_MICROBIT_CONNECTION") {
    background(220);
    textAlign(CENTER, CENTER);
    textSize(24);
    text("Conecta el micro:bit para comenzar", width / 2, height / 2);
    connectBtn.show();
    return;
  }

  connectBtn.hide();

  // procesar datos binarios
  if (port.opened() && port.availableBytes() > 0) {
    readBinaryPacket();
  }

  // Estado RUNNING
  background(255);
  randomSeed(actRandomSeed);

  translate(tileWidth / 2, tileHeight / 2);

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

      for (let i = 0; i < circleCount; i++) {
        let diameter = map(i, 0, circleCount, tileWidth, endSize);
        let offset = map(i, 0, circleCount, 0, endOffset);
        ellipse(offset, 0, diameter, diameter);
      }
      pop();
    }
  }

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
    state = "RUNNING";
  } else {
    port.close();
    state = "WAIT_MICROBIT_CONNECTION";
    connectBtn.html("Conectar micro:bit");
  }
}

function readBinaryPacket() {
  while (port.availableBytes() > 0) {
    let byte = port.readByte();

    if (byte === -1) return; // no data

    packetBuffer.push(byte);

    // buscar encabezado
    if (packetBuffer[0] !== 0xAA) {
      packetBuffer.shift();
      continue;
    }

    // esperar paquete completo
    if (packetBuffer.length >= 8) {
      let packet = packetBuffer.slice(0, 8);
      packetBuffer = [];

      // verificar checksum
      let dataBytes = packet.slice(1, 7);
      let receivedChecksum = packet[7];
      let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;

      if (receivedChecksum !== computedChecksum) {
        console.warn("Checksum incorrecto");
        return;
      }

      // reconstruir valores
      let xHigh = dataBytes[0];
      let xLow = dataBytes[1];
      let yHigh = dataBytes[2];
      let yLow = dataBytes[3];

      // big-endian porque usamos '>2h2B'
      let rawX = (xHigh << 8) | (xLow & 0xFF);
      let rawY = (yHigh << 8) | (yLow & 0xFF);

      // convertir a signed 16-bit
      if (rawX & 0x8000) rawX = rawX - 0x10000;
      if (rawY & 0x8000) rawY = rawY - 0x10000;

      microBitX = rawX;
      microBitY = rawY;
      buttonA = dataBytes[4] === 1;
      buttonB = dataBytes[5] === 1;
    }
  }
}

```
