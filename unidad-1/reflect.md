# Unidad 1
## 🤔 Fase: Reflect

### Actividad 7  

### Parte 1:

#### 1.

Un sistema físico interactivo es una forma de relacionarme y combinar el entorno con los computadores. Por ejemplo, si presiono un botón físico, eso puede activar algo en el software o en el computador. Tiene tres partes principales:

- **Inputs:** Son las acciones o información que entran al sistema, como presionar un botón o mover un objeto.
- **Procesamiento:** El sistema interpreta esas acciones y decide qué hacer con ellas.
- **Outputs:** Es la reacción visible o física del sistema después de procesar la entrada, como mover algo en pantalla o encender una luz.


#### 2.

#### En el **micro:bit**:

- **Input:** Presionar los botones A y B.
- **Procesamiento:** Detectar cuál botón fue presionado y decidir qué letra enviar por el puerto serial.
- **Output:** Enviar la letra correspondiente (por ejemplo, 'A' o 'B') al computador.

#### En el **computador (p5.js)**:

- **Input:** Recibir la letra que mandó el micro:bit por serial.
- **Procesamiento:** El programa lee esa letra y decide qué hacer según cuál fue.
- **Output:** El círculo en la pantalla se mueve en la dirección correspondiente (izquierda o derecha).


#### 3.

- La línea `uart.write('A')` en el código del micro:bit **envía un mensaje por comunicación serial al computador**, en este caso, la letra `'A'`.

- En p5.js, la función que se encarga de "escuchar" o recibir ese mensaje es:

```javascript
serial.on('data', gotData);
```

Aquí, `gotData` es una función que se ejecuta cada vez que llega un mensaje desde el micro:bit.


#### 4.

La diferencia más clara es que en el arte o diseño tradicional uno hace todo a mano, como dibujar, pintar o diseñar algo directamente. En cambio, en el arte o diseño generativo, uno programa un sistema o un código que **crea cosas automáticamente**, a veces de forma aleatoria o con ciertos parámetros. Entonces no estás haciendo cada parte directamente, sino que le das instrucciones al programa para que lo genere, y muchas veces los resultados cambian cada vez.


#### 5.

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


### Parte 2

#### 1.

Definitivamente lo más desafiante para mí fue la parte técnica, especialmente lograr que el micro:bit se comunicara bien con p5.js. Hacer que el código funcionara a la perfección fue bastante complejo, sobre todo porque a veces no entendía del todo cómo funcionaban las conexiones. Pero con el paso de las clases y los ejercicios, fui entendiendo mejor y logré avanzar.


#### 2.

El momento “¡Aha!” lo tuve cuando por fin presioné un botón en el micro:bit y el círculo en el canvas de p5.js se movió. Fue como: *¡Wooow, funcionó!*. En ese momento entendí que había una conexión real entre lo físico y lo digital. No solo estaba escribiendo código por separado, sino que lo que yo tocaba físicamente se estaba viendo reflejado en una pantalla. Sentí que realmente estaba creando una experiencia interactiva.


#### 3.

Al principio no tenía tan claro para qué me iba a servir este curso, pero después de construir el prototipo, me di cuenta de que esto abre muchas posibilidades creativas y profesionales. Ahora entiendo que no solo se trata de hacer cosas visuales o físicas por separado, sino de combinarlas para crear experiencias que respondan al usuario. Me sirve para pensar en instalaciones interactivas, productos digitales que se conecten con el cuerpo o el entorno, y proyectos artísticos con tecnología.


#### 4.

El tutorial fue muy bueno, aunque comparado con la actividad real sí noté algunas diferencias. Aun así, seguir el paso a paso me ayudó mucho a entender la estructura del código y me dio seguridad. Después de hacerlo una vez, ya me dieron ganas de probar cosas por mi cuenta y modificarlo a mi estilo.  

### Actividad 8.

- Qué diferencias hay?
  
Mi compañero manejó la conexión del micro:bit con un botón que cambia de texto entre “Connect to micro:bit” y “Disconnect”, lo cual me pareció muy útil porque da más claridad al usuario. Además, usó una variable booleana llamada "connectionInitialized" para asegurarse de limpiar el puerto solo una vez cuando se conecta, lo que hace que el código sea más estable y no se repita esa acción muchas veces.

Otra diferencia importante es cómo estructura la lectura de los datos: primero verifica si hay datos disponibles (port.availableBytes() > 0) y luego lee solo un byte con port.read(1), mientras que yo tal vez lo tenía más directo y simple, sin ese control.

- Qué aprendi?
  
Aprendí que tener un botón con un texto dinámico puede mejorar bastante la experiencia del usuario. También entendí que limpiar el puerto una sola vez al conectar evita errores raros o comportamientos extraños que a veces pasaban en mi código. Me gustó mucho su forma de organizar las funciones y mantener el control de la conexión en una sola parte del código.

- Qué hice yo?
  
Mi código también recibía letras desde el micro:bit (por ejemplo, ‘A’ o ‘B’ si se presionaban los botones), y hacía que el círculo se moviera a la izquierda o derecha. Pero mi versión era un poco más sencilla: no usé un botón para conectar, sino que abría el puerto directamente o dependía más del ejemplo básico. Tampoco tenía una validación tan clara del estado de la conexión.

Mi enfoque funcionó, pero no era tan robusto ni tan organizado como el de mi compañero. Me di cuenta de que vale la pena tomarse el tiempo de hacer que el código sea más claro y que tenga controles más completos.


### Actividad 9  

#### 1. Continuar:
El video del teatro, donde el logo cambiaba según la música, me pareció muy impactante. Fue visualmente impresionante y además me ayudó a entender mejor cuál es el verdadero potencial de los sistemas físicos interactivos. Me inspiró mucho porque mostró cómo se pueden generar experiencias muy inmersivas con tecnología y arte.

#### 2. Dejar de hacer:
En general, la forma en que se ha dado el tema me parece muy buena, clara y bien estructurada. Sin embargo, siento que las actividades 5 y 6 podrían tener un poco más de tiempo o acompañamiento, ya que son más complejas de entender, sobre todo al momento de lograr la comunicación entre el micro:bit y p5.js. A veces se sintió un poco rápido y eso puede dificultar el proceso.

#### 3. Empezar a hacer:
Me genera mucha curiosidad explorar interacciones más complejas, especialmente con sensores como el de luz y temperatura. Me gustaría ver ejemplos más avanzados o creativos como el del teatro, donde se combinan varias entradas y salidas para lograr efectos visuales impactantes.

#### 4. Balance inspiración vs. técnica:
Siento que hubo un buen equilibrio entre la parte inspiradora, como los videos y ejemplos de la Actividad 01, y la parte técnica de las actividades 03 a 06. Empezar con inspiración me ayudó a entender por qué estamos aprendiendo esto, y luego la parte técnica me permitió materializar esas ideas.

#### 5. Comentario adicional:
En general me gustó mucho esta unidad. Me ayudó a conectar ideas técnicas con conceptos más artísticos, y me emociona seguir explorando nuevas formas de interacción entre el mundo físico y el digital.




