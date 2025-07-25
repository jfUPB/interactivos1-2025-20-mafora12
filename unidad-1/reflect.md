# Unidad 1
## 🤔 Fase: Reflect

### Actividad 7  

Parte1:

 1. Un sistema fisico interactivo es una forma de relacionarme y combinar el entorno con los computadores, así si undo un boton afuera hace algo en el software o computador. Tiene tres partes que son:
     
- Los inputs: son las acciones o información entran al software
- los outputs: LA reacción del sistema a los outputs
- Procesamiento: interpreta las acciones.

2. 

Micro:bit:

- Los inputs: Undir botones A y B
- Los outputs:Lainformación de cual boton se envio al puerto serial
- Procesamiento: detectaba cuál botón fue presionado y decidía qué letra enviar por el cable.
  
Computador:  

- Los inputs: recibír la letra que mandó el micro:bit
- Procesamiento: programa leía esa letra y decidía qué hacer según cuál había llegado
- Output: El círculo en la pantalla se movía en la dirección correspondiente.

3. 
- La línea uart.write('A') en el código del micro:bit envía un mensaje por comunicación serial al computador, en este caso la letra 'A'.
  
- En p5.js, la función que se encarga de "escuchar" o recibir ese mensaje es:
serial.on('data', gotData);

Aquí, gotData es una función que se ejecuta cada vez que llega un mensaje desde el micro:bit.  

4.
La diferencia más clara es que en el arte o diseño tradicional uno hace todo a mano, como dibujar, pintar o diseñar algo directamente. En cambio, en el arte o diseño generativo, uno programa un sistema o un código que crea cosas automáticamente, a veces de forma aleatoria o con ciertos parámetros. Entonces uno no está haciendo cada cosa directamente, sino que le dice al programa cómo lo haga, y muchas veces los resultados cambian cada vez.


5.

micro:bit:  

```javascript
from microbit import *
import uart

uart.init()

while True:
    if accelerometer.was_gesture('shake'):
        uart.write('S')
```

p5:  
```javascriptlet circleColor;

function setup() {
  createCanvas(400, 400);
  circleColor = color(255);
  serial = new p5.SerialPort();
  serial.open('/dev/ttyUSB0'); // este puerto depende de cada compu
  serial.on('data', gotData);
}

function gotData() {
  let input = serial.read();
  if (input === 'S') {
    circleColor = color(random(255), random(255), random(255));
  }
}

function draw() {
  background(200);
  fill(circleColor);
  ellipse(width / 2, height / 2, 100, 100);
}

```

