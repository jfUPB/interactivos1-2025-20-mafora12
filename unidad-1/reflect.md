# Unidad 1
## 🤔 Fase: Reflect

### Actividad 7  

Parte 1:  

1.   
Un sistema fisico interactivo es una forma de relacionarme y combinar el entorno con los computadores, así si undo un boton afuera hace algo en el software o computador. Tiene tres partes que son:
     
- Los inputs: son las acciones o información entran al software
- los outputs: LA reacción del sistema a los outputs
- Procesamiento: interpreta las acciones.

2.  

- Micro:bit:
 - Los inputs: Undir botones A y B
 - Los outputs:Lainformación de cual boton se envio al puerto serial
 - Procesamiento: detectaba cuál botón fue presionado y decidía qué letra enviar por el cable.
  
- Computador:  
 - Los inputs: recibír la letra que mandó el micro:bit
 - Procesamiento: programa leía esa letra y decidía qué hacer según cuál había llegado
 - Output: El círculo en la pantalla se movía en la dirección correspondiente.

3.  
 - La línea uart.write('A') en el código del micro:bit envía un mensaje por comunicación serial al computador, en este caso la letra 'A'.
  
 - En p5.js, la función que se encarga de "escuchar" o recibir ese mensaje es:
   

    ```javascript
        serial.on('data', gotData);
    ```  

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

Parte 2:  
1.  
Definitivamente lo más desafiante para mí fue la parte técnica, especialmente lograr que el micro:bit se comunicara bien con p5.js. Que el codigo funcione a la perfección fue bastante complejo, más si a vces no entendia mucho.Pero con el paso de las calses y los ejercicios fui mejorando.  


2.  

El momento “¡Aha!” lo tuve cuando por fin presioné un botón en el micro:bit y el círculo en el canvas de p5.js se movió. Fue como: ¡Wooow, funcionó! Ahí entendí de verdad que había una conexión real entre lo físico y lo digital. No solo estaba escribiendo código por separado, sino que estaba haciendo que algo que yo tocaba se viera reflejado en una pantalla.


3.
Al principio no tenía tan claro para qué me iba a servir este curso, pero después de construir el prototipo, me di cuenta de que esto abre muchas posibilidades creativas y profesionales. Ahora entiendo que no solo se trata de hacer cosas visuales o físicas por separado, sino de combinarlas para crear experiencias que respondan al usuario. Me sirve para pensar en instalaciones interactivas, productos digitales que se conecten con el cuerpo o el entorno.

4.
El tutorial fue muy bueno, aunque comparado con la actividad real sí noté algunas diferencias. Aun así, seguir el paso a paso me ayudó mucho a entender la estructura del código y me dio seguridad. Después de hacerlo una vez, ya me dieron ganas de probar cosas por mi cuenta y modificarlo a mi estilo.
