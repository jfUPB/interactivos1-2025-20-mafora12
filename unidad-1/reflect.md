# Unidad 1
## 🤔 Fase: Reflect

### Actividad 7  

### 1.

Un sistema físico interactivo es una forma de relacionarme y combinar el entorno con los computadores. Por ejemplo, si presiono un botón físico, eso puede activar algo en el software o en el computador. Tiene tres partes principales:

- **Inputs:** Son las acciones o información que entran al sistema, como presionar un botón o mover un objeto.
- **Procesamiento:** El sistema interpreta esas acciones y decide qué hacer con ellas.
- **Outputs:** Es la reacción visible o física del sistema después de procesar la entrada, como mover algo en pantalla o encender una luz.


### 2.

#### En el **micro:bit**:

- **Input:** Presionar los botones A y B.
- **Procesamiento:** Detectar cuál botón fue presionado y decidir qué letra enviar por el puerto serial.
- **Output:** Enviar la letra correspondiente (por ejemplo, 'A' o 'B') al computador.

#### En el **computador (p5.js)**:

- **Input:** Recibir la letra que mandó el micro:bit por serial.
- **Procesamiento:** El programa lee esa letra y decide qué hacer según cuál fue.
- **Output:** El círculo en la pantalla se mueve en la dirección correspondiente (izquierda o derecha).


### 3.

- La línea `uart.write('A')` en el código del micro:bit **envía un mensaje por comunicación serial al computador**, en este caso, la letra `'A'`.

- En p5.js, la función que se encarga de "escuchar" o recibir ese mensaje es:

```javascript
serial.on('data', gotData);
```

Aquí, `gotData` es una función que se ejecuta cada vez que llega un mensaje desde el micro:bit.


### 4.

La diferencia más clara es que en el arte o diseño tradicional uno hace todo a mano, como dibujar, pintar o diseñar algo directamente. En cambio, en el arte o diseño generativo, uno programa un sistema o un código que **crea cosas automáticamente**, a veces de forma aleatoria o con ciertos parámetros. Entonces no estás haciendo cada parte directamente, sino que le das instrucciones al programa para que lo genere, y muchas veces los resultados cambian cada vez.


### 5.

#### Código en **micro:bit**:

```python
from microbit import *
import uart

uart.init()

while True:
    if accelerometer.was_gesture('shake'):
        uart.write('S')
```

#### Código en **p5.js**:

```javascript
let circleColor;

function setup() {
  createCanvas(400, 400);
  circleColor = color(255);
  serial = new p5.SerialPort();
  serial.open('/dev/ttyUSB0'); // este puerto depende de cada computador
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


## Parte 2

### 1.

Definitivamente lo más desafiante para mí fue la parte técnica, especialmente lograr que el micro:bit se comunicara bien con p5.js. Hacer que el código funcionara a la perfección fue bastante complejo, sobre todo porque a veces no entendía del todo cómo funcionaban las conexiones. Pero con el paso de las clases y los ejercicios, fui entendiendo mejor y logré avanzar.


### 2.

El momento “¡Aha!” lo tuve cuando por fin presioné un botón en el micro:bit y el círculo en el canvas de p5.js se movió. Fue como: *¡Wooow, funcionó!*. En ese momento entendí que había una conexión real entre lo físico y lo digital. No solo estaba escribiendo código por separado, sino que lo que yo tocaba físicamente se estaba viendo reflejado en una pantalla. Sentí que realmente estaba creando una experiencia interactiva.


### 3.

Al principio no tenía tan claro para qué me iba a servir este curso, pero después de construir el prototipo, me di cuenta de que esto abre muchas posibilidades creativas y profesionales. Ahora entiendo que no solo se trata de hacer cosas visuales o físicas por separado, sino de combinarlas para crear experiencias que respondan al usuario. Me sirve para pensar en instalaciones interactivas, productos digitales que se conecten con el cuerpo o el entorno, y proyectos artísticos con tecnología.


### 4.

El tutorial fue muy bueno, aunque comparado con la actividad real sí noté algunas diferencias. Aun así, seguir el paso a paso me ayudó mucho a entender la estructura del código y me dio seguridad. Después de hacerlo una vez, ya me dieron ganas de probar cosas por mi cuenta y modificarlo a mi estilo.
