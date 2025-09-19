
# Evidencias de la unidad 5

# Evidencias de la unidad 5

## Actividad 2

### ¿Qué datos envía el micro:bit?  
El micro:bit envía datos al computador (y, en este caso, al sketch en p5.js) a través del puerto serie utilizando la interfaz UART. En cada ciclo, aproximadamente cada 100 ms, el micro:bit realiza varias lecturas internas y construye un mensaje en formato de texto:

Lecturas del acelerómetro: obtiene la posición en los ejes X y Y, que permiten interpretar inclinaciones o movimientos básicos del dispositivo.

Estado de los botones A y B: determina si están presionados (generalmente 1) o no (0).

Con esta información, el micro:bit genera una cadena de caracteres en formato ASCII, donde los valores se separan por comas y terminan con un salto de línea \n. El código típico sería:

``` js
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
uart.write(data)
```
La elección de ASCII tiene ventajas y desventajas:  
- Ventaja: es legible para humanos. Con un simple monitor serial podemos inspeccionar los valores y depurar errores fácilmente.  
- Desventaja: no es eficiente en cuanto a memoria ni velocidad, porque cada número se envía carácter por carácter (por ejemplo, 123 ocupa tres bytes en ASCII, pero en binario solo se necesitaría un byte).  

Esto abre la posibilidad de que, en proyectos que necesiten enviar datos más rápidos o grandes volúmenes (como sensores múltiples o transmisión continua), el protocolo ASCII se quede corto y sea necesario migrar a un protocolo binario.  

### ¿Cómo es la estructura del protocolo ASCII usado?
La estructura del protocolo ASCII que usa el micro:bit es bastante sencilla y fácil de leer, justamente porque está pensada para ser entendida rápidamente tanto por la computadora como por una persona si abre un monitor serial. El mensaje que se envía sigue siempre este orden:   

``` js
xValue, yValue, aState, bState\n
```

"xValue, yValue" -> son números enteros que representan las lecturas del acelerómetro en los ejes X e Y.    

"aState, bState"-> son valores lógicos (0 o 1) que indican si los botones A y B están presionados o no.   

"," → funciona como separador de cada valor, de modo que cuando el mensaje llega al sketch de p5.js se puede “delimitar” fácilmente en partes.  

"\n" → marca el final del paquete, y ayuda a que el receptor sepa que ya terminó un mensaje y puede procesarlo.  

Este sistema es útil porque es claro y humano-legible: cualquiera puede abrir un monitor serial y reconocer los datos a simple vista. Sin embargo, también tiene limitaciones. Por ejemplo, si alguna coma se pierde o se corrompe el mensaje, el receptor puede confundir los valores y todo el paquete queda “descuadrado”. Además, al usar caracteres ASCII en lugar de binario, el envío ocupa más espacio y no aprovecha al máximo la velocidad de transmisión.  

En pocas palabras, se priorizó la claridad sobre la eficiencia. En un proyecto académico como este o experimental esto funciona muy bien, porque es más sencillo aprender y depurar. Pero en proyectos que requieran rapidez o grandes volúmenes de datos, se haría evidente que esta estructura no es la más óptima.  

#### Duda:  <a name="cp1"></a>
¿Qué pasaría si quisiera enviar más información, como los tres ejes completos del acelerómetro (X, Y, Z)?    
  - Respuesta: 
    Si quisiera enviar los tres ejes del acelerómetro (X, Y y Z) usando ASCII, lo que tendría que hacer es ampliar la cadena de texto. En lugar de solo dos valores separados por comas, el mensaje quedaría algo así:
    
    ``` js
    xValue,yValue,zValue,aState,bState\n
    ```
    
    De esa forma cada dato seguiría siendo legible en cualquier monitor serial, porque se verían los números completos como texto. La desventaja es que el mensaje sería más largo, ocuparía más espacio y tardaría un poquito más en llegar,        porque cada número en ASCII usa varios caracteres en vez de un solo byte.  
    El programa receptor también tendría que actualizarse: en lugar de dividir solo 4 valores, ahora tendría que dividir 5 (o incluso más, si agrego otros sensores).  
    
### Lectura y transformación de los datos en p5.js    
La función draw() en p5.js es como el puente entre lo que manda el micro:bit y lo que finalmente vemos en la pantalla. Dentro de ella no solo se dibujan cosas, también se verifica constantemente si hay datos nuevos en el puerto serie. Cuando detecta que llegaron bytes, los lee hasta encontrar el salto de línea (\n). Ese salto es clave, porque marca el final del paquete y garantiza que lo que se reciba esté completo.  

Una vez que se tiene la línea entera, se limpia con .trim() para quitar espacios o caracteres extra, y luego se divide usando split(","). Esto convierte la cadena en un arreglo con las cuatro partes: xValue, yValue, aState, y bState. Después se hace una validación importante: si efectivamente llegaron los cuatro valores esperados, se procede a convertirlos en el tipo de dato correcto:  

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

- "xValue y yValue" se transforman en enteros y se suman a la mitad del ancho y del alto de la ventana. Esto no es al azar, sino para centrar los valores del micro:bit dentro del lienzo de p5.js, de modo que cualquier inclinación se refleje en coordenadas de la pantalla.  

- "aState y bState" se convierten en booleanos (true o false) comparando el texto recibido. Luego se pasa esta información a la función updateButtonStates() para que actualice la lógica del programa (por ejemplo, encender algo o mover un objeto si un botón está presionado).  

Este proceso muestra que el protocolo no solo organiza la información en el micro:bit, sino que además p5.js la interpreta en un orden exacto. Si llegara algo desordenado, duplicado o incompleto, el sistema podría comportarse mal. Por eso es fundamental que ambos extremos “hablen el mismo idioma” y respeten la estructura del mensaje.
En conclusión, draw() no es solo un lugar para dibujar, sino también un espacio donde se traduce y valida la comunicación: convierte simples caracteres ASCII en acciones concretas dentro de la pantalla.  

#### Dudas:   
- ¿Qué ocurriría si por error el micro:bit enviara cinco valores en vez de cuatro? ¿El sketch los ignoraría, se dañaría la animación o debería implementar un sistema más robusto de validación?  
    - Respuesta:  
      Si el micro:bit llegara a mandar cinco valores en lugar de cuatro, el programa en p5.js se confundiría porque está preparado solo para recibir cuatro. Al hacer el split(","), se generarían más partes de las que el código espera y ahí se podría dañar la animación o usar datos equivocados. Básicamente, el quinto valor quedaría sobrando y no tendría a dónde ir.
      Para evitar eso, lo mejor es poner una condición que revise si realmente llegaron exactamente cuatro valores antes de procesarlos. Si no se cumple, simplemente se ignora ese paquete y así el programa no se rompe.
      
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
      

- ¿Y si los datos llegan incompletos porque hubo ruido en la transmisión? ¿Cómo podría detectar que un paquete viene corrupto y descartarlo en lugar de usarlo?   
  - Respuesta:  
    Cuando los datos no llegan completos (por ejemplo, falta una parte del número o se corta el mensaje), el programa puede intentar procesarlos igual y eso generaría errores o valores sin sentido en la animación.  
    Para evitarlo, se pueden hacer varias cosas sencillas, como:   
     - Revisar que el mensaje siempre termine en \n.    
     - Confirmar que la cantidad de datos separados por comas sea la correcta.   
     - Y si quisiera ponerlo más seguro, usar algún tipo de marca o comprobación extra que me diga si el mensaje está completo.  

### ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?    

Los eventos como “A pressed” o “B released” no aparecen mágicamente en p5.js, sino que son el resultado de una comparación lógica entre el estado actual de los botones y el estado que tenían en el ciclo anterior. La función updateButtonStates() es la encargada de hacer ese chequeo.  

Cuando el micro:bit envía los valores de los botones en ASCII, p5.js los guarda como booleanos (true si está presionado, false si no lo está). Luego, en cada actualización del draw(), updateButtonStates() compara esos nuevos valores con los que tenía guardados en las variables prevmicroBitAState y prevmicroBitBState.    

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

- Si el botón A estaba suelto (false) y de repente pasa a estar presionado (true), se detecta la transición de suelto a presionado. En ese instante se ejecutan las acciones asociadas: cambiar el tamaño de un módulo y guardar la posición del click, además de imprimir en consola “A pressed”.  

- Si el botón B estaba presionado (true) y ahora aparece como suelto (false), se detecta la transición contraria de presionado a liberado. Eso dispara un cambio de color en la visualización y muestra “B released” en consola.   

La clave está en que no se revisa solo el valor actual, sino el cambio respecto al anterior. Gracias a esa lógica de transición, se pueden generar eventos puntuales (como presionar o soltar) en lugar de que la acción se repita constantemente mientras el botón esté en cierto estado.    
En resumen, el micro:bit solo manda datos simples (0 o 1), pero en p5.js esos datos se interpretan de manera más rica: se convierten en eventos gráficos concretos que responden al momento exacto en que ocurre una interacción.   

#### Duda:  

Si quisiera detectar también el evento de ‘mantener presionado’ (por ejemplo, que algo cambie continuamente mientras el botón A sigue en true), ¿cómo debería modificar la lógica de updateButtonStates() para diferenciar entre una transición y un estado sostenido?   
  - Respuesta:    
  Si yo quisiera que también se detecte cuando el botón A está sostenido (no solo cuando se presiona o se suelta), tendría que cambiar la lógica de updateButtonStates(). Ahora solo revisa los cambios de estado, o sea, si pasó de false a       true (presionado) o de true a false (soltado). Pero no está mirando qué pasa si el botón sigue en true durante varios frames seguidos.  
  Para eso, lo que hago es meter otra condición que diga: “si el botón A sigue en true, haz esto”. Así no depende de un cambio, sino que se ejecuta mientras el botón esté apretado.       
  
  ``` js
function updateButtonStates(newAState, newBState) {
  // --- Botón A ---
  // Evento: cuando A acaba de ser presionado
  if (newAState === true && prevmicroBitAState === false) {
    lineModuleSize = random(50, 160);
    clickPosX = microBitX;
    clickPosY = microBitY;
    print("A pressed");
  }

  // Estado sostenido: mientras A siga presionado
  if (newAState === true) {
    // aquí pones lo que debe repetirse mientras A esté en true
    print("A is being held down");
  }

  // --- Botón B ---
  // Evento: cuando B acaba de ser soltado
  if (newBState === false && prevmicroBitBState === true) {
    c = color(random(255), random(255), random(255), random(80, 100));
    print("B released");
  }

  // Actualizar estados previos
  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}

```   

## Dibujos
<img width="1918" height="937" alt="image" src="https://github.com/user-attachments/assets/277e9abb-df2d-40ea-a488-e8ad51ad0558" />
<img width="1918" height="932" alt="image" src="https://github.com/user-attachments/assets/c8080c4b-e7cf-491f-afc4-ff358cf58ae9" />

### Dudas
- ¿Qué pasaría si en lugar de usar delimitadores para separar datos, se usara un tamaño fijo para cada valor?
  Si todo tuviera un tamaño fijo sería más fácil leer y decodificar porque ya sabes dónde empieza y termina cada dato. El problema es que puede desperdiciar         espacio si los números son muy pequeños o que se quede corto si uno es demasiado grande. Entonces, ¿vale la pena sacrificar flexibilidad por simplicidad?

- ¿Cómo cambiaría la complejidad del sistema si además de los datos incluyera un checksum o un byte de validación?
  Tener un checksum haría que el sistema fuera más robusto porque podría detectar si algo se dañó en la transmisión. Pero al mismo tiempo se agregan más pasos       para calcularlo y verificarlo. Es como más seguro pero también más trabajo. Entonces uno se pregunta: ¿qué pesa más, la seguridad o la simplicidad?
  
## Actividad 3

### ¿Por qué se ve este resultado?
<img width="1233" height="552" alt="image" src="https://github.com/user-attachments/assets/291af6f7-14cb-4e74-90a1-51cfda9efa55" />  

Lo que se ve en la imagen con tantos símbolos extraños ocurre porque hay un choque entre el tipo de datos enviados y la forma en que se están mostrando. El micro:bit está enviando los valores en binario (por ejemplo, usando struct.pack), mientras que la aplicación está configurada para mostrar lo recibido como si fueran caracteres de texto (ASCII o UTF-8).  
El problema es que esos bytes binarios no corresponden a letras, números o símbolos “legibles” en una codificación de texto, así que el programa intenta interpretarlos a la fuerza y aparecen como una especie de caracteres rotos, cuadrados o símbolos sin sentido.  

<img width="1237" height="407" alt="image" src="https://github.com/user-attachments/assets/8103990e-a211-4551-9f94-ca3968b10218" />  
Aquí lo que realmente pasa es que los valores del acelerómetro y los botones no se mandan “sueltos” ni en números fáciles de leer, sino que se empaquetan en formato binario: dos enteros de 16 bits y dos bytes más. En la captura, cada par de caracteres en hexadecimal equivale a un byte de esos datos comprimidos. O sea, lo que se ve son bloques de información cruda, y toca ir decodificándolos para entender cuál parte corresponde a cada sensor o botón.  
Lo raro es que, a diferencia de cuando los mandamos en ASCII (donde se ven números completos, separados por comas o espacios), aquí ya no tenemos una lectura directa. Ahora toca preguntarse: ¿cómo sé exactamente qué grupo de bytes corresponde al eje X del acelerómetro, o cuál byte está indicando el estado de un botón? ¿Y si el orden cambia, cómo lo detecto sin confundirme?

- Ventajas del binario

  - Ocupa mucho menos espacio, porque no se mandan caracteres extra como comas, saltos de línea o espacios. Solo lo esencial.

  - Al ser más compacto, también es mucho más rápido de enviar y recibir, sobre todo cuando hay grandes cantidades de datos en tiempo real.

  - Además, como no hay caracteres “intermedios” que puedan perderse, se reducen ciertos errores de interpretación.

- Desventajas del binario

  - Es casi ilegible a simple vista. Si miro la captura, no tengo ni idea de lo que significa cada bloque de bytes sin una guía.

  - Necesito conocer exactamente la estructura con la que fueron empaquetados los datos, de lo contrario no hay forma de traducirlos bien.

  - A la hora de depurar o hacer pruebas rápidas, se vuelve poco práctico. Es casi obligatorio usar herramientas que conviertan esos bytes en algo legible.
    
<img width="1263" height="577" alt="image" src="https://github.com/user-attachments/assets/d4e4902b-9afe-4518-9e40-913d285a5a69" />
En esta captura lo que vemos son los datos ya empaquetados y transmitidos en crudo. El formato '>2h2B' nos indica la “receta” para interpretar esos bytes:  
  ">" → significa que se está usando big-endian, es decir, que los bytes más significativos se envían primero.
  "2h" → son dos enteros de 16 bits con signo (2 bytes cada uno).  
  "2B" → son dos enteros de 8 bits sin signo (1 byte cada uno).  
  En total, cada mensaje ocupa 6 bytes exactos, siempre en el mismo orden.  
Entonces, la estructura queda así:  
  - xValue → 2 bytes.  
  - yValue → 2 bytes.  
  - aState (botón A) → 1 byte.  
  - bState (botón B) → 1 byte.  
Interpretación byte a byte:    
Los primeros 2 bytes corresponden a xValue. Como es un entero con signo, puede ser positivo o negativo.  
Los siguientes 2 bytes son el yValue, con la misma lógica.  
El quinto byte es el estado del botón A: 00 si no está presionado, 01 si lo está.    
El sexto byte es el estado del botón B, con el mismo criterio.  
Complemento a dos (para los enteros con signo):   
Como los valores de X e Y son enteros de 16 bits con signo, cuando un número es negativo se representa en complemento a dos.  

Ejemplos:  
Si xValue = 100, en hexadecimal se vería como: 00 64.  
Si xValue = -100, en hexadecimal aparecería como: FF 9C.  
Esto mismo aplica para yValue. En cambio, los botones nunca serán negativos, por eso se usan enteros sin signo de 8 bits: siempre 00 o 01.   

#### Duda:
¿Qué tan seguro es confiar en que cada paquete siempre llega con 6 bytes exactos? ¿Qué pasaría si en medio de la transmisión se pierde un byte? ¿Se “descuadra” toda la interpretación?     
  - Respuesta:  
    No es del todo seguro confiar en que siempre lleguen los 6 bytes justos, porque en la transmisión puede pasar que se pierda un byte o llegue de más. Si eso pasa, todo el orden de los datos se corre y la lectura se “descuadra”, o sea que     empiezas a interpretar mal cada valor. Por eso es mejor tener alguna validación o marca que te ayude a saber cuándo empieza y termina un paquete, así puedes descartar los dañados en vez de usarlos mal.  
      ``` js
      let buffer = [];

      function onDataReceived(byte) {
      // Ir guardando los bytes que llegan
      buffer.push(byte);

      // Si ya tenemos 6 bytes, revisamos
      if (buffer.length === 6) {
    // Aquí podrías validar que empiece con un "marcador" fijo, por ejemplo 0xAA
    if (buffer[0] === 0xAA) {
      // Paquete válido → usarlo
      processData(buffer);
    } else {
      // Paquete corrupto → descartar
      console.log("Paquete inválido, descartado");
    }
    // Vaciar para el siguiente paquete
    buffer = [];
      }
    }

    function processData(packet) {
    // Aquí interpretas los 6 bytes como quieras
    console.log("Paquete válido:", packet);
    }
    ```    
    La idea es:  
    Vas acumulando los bytes que llegan. Cuando tienes 6, revisas si cumplen una condición (ejemplo: empiezan con un byte fijo como 0xAA). Si no cumplen → descartas y sigues esperando el siguiente. Así evitas que la lectura se “descuadre”.

<img width="1230" height="541" alt="image" src="https://github.com/user-attachments/assets/468cc386-595e-476b-ac0d-fa3fd4d534a9" />  
En esta captura se ve claramente la diferencia entre datos enviados en binario y en ASCII.
Cuando los datos viajan en binario (HEX), aparecen como bytes crudos. Cada par de caracteres hexadecimales representa un byte real en memoria. Eso hace que la transmisión sea mucho más eficiente, pero a simple vista no se entiende nada si no conoces el formato exacto (por ejemplo, >2h2B en este caso).
En cambio, cuando los datos viajan en ASCII, lo que recibimos son números y texto legibles directamente: por ejemplo, -356, 572, 0, 1. Aquí no necesitas saber nada sobre la estructura, solo leerlo como si fuera texto.

Formato binario  

- Ventajas  
  - Usa el tamaño real de cada valor: enteros de 16 bits o 8 bits, sin desperdiciar espacio.

  - Es más rápido de enviar y recibir, porque se mandan menos bytes.

  - Mantiene la precisión de los números negativos y positivos gracias al complemento a dos.

- Desventajas

  - Para interpretarlo hay que conocer la estructura exacta: orden de los campos, tamaño de cada valor, endianess.

  - Muy difícil de depurar sin herramientas adicionales.

  - Si se pierde un solo byte en la transmisión, se desordena todo el paquete.
Formato ASCII

- Ventajas

  - Fácil de leer con cualquier terminal: incluso sin programa especial, puedes “ver” los números.

  - No necesitas saber el formato, porque el separador de comas y los saltos de línea ya organizan los datos.

  - Es universal: cualquier software que lea texto lo interpreta sin problema.

- Desventajas

  - Ocupa mucho más espacio: por ejemplo, el número 572 en ASCII ocupa 3 bytes ('5' '7' '2'), mientras que en binario solo ocuparía 2 bytes.

  - Es más lento de enviar y procesar, sobre todo cuando hay gran cantidad de datos en tiempo real.

  - Requiere convertir de texto a número cada vez que se quiere usar en cálculos.


#### Duda:
- ¿Qué pasaría si un número en ASCII es muy grande y ocupa más bytes que su equivalente en binario? ¿Vale la pena ese costo en comunicación?  
    - Respuesta:  
      Si un número se manda en ASCII, puede terminar ocupando varios bytes solo para representarlo (por ejemplo, el número 12345 son 5 caracteres, o sea 5 bytes), mientras que en binario ese mismo valor cabría en 2 bytes. Eso significa que        en ASCII gastas más espacio y la comunicación se vuelve más pesada. Entonces toca pensar si vale la pena esa claridad de lectura a cambio de gastar más ancho de banda.   

- En sistemas críticos (como sensores en tiempo real), ¿realmente conviene arriesgarse a usar ASCII sabiendo que puede retrasar la transmisión?  
    - Respuesta:  
      En sistemas críticos, como sensores que tienen que mandar datos en tiempo real y sin retrasos, usar ASCII puede ser un riesgo porque hace más lenta la transmisión. En esos casos casi siempre es mejor usar binario, ya que es más              eficiente y seguro para no perder tiempo ni precisión.  

## Actividad 3  

### Explica:   
Antes, en la unidad pasada, todo se manejaba con ASCII, lo que implicaba que cada número podía variar en cantidad de caracteres. Por eso tocaba meter comas y saltos de línea para que el receptor supiera dónde terminaba cada dato y dónde empezaba otro. Era como mandar un mensaje de texto con separadores, y luego reconstruirlo pedacito por pedacito.  
Ahora, con struct.pack(), cada valor se convierte en un byte o bloque fijo:

- 2 bytes para xValue.  

- 2 bytes para yValue.  

- 1 byte para aState.  

- 1 byte para bState.  
Eso significa que el paquete siempre pesa lo mismo: 6 bytes. Ya no tengo que preocuparme por delimitadores ni saltos de línea porque cada bloque está en una posición definida. El receptor sabe exactamente dónde mirar para sacar cada valor.

### ¿Qué cambios observas?  

La transición es clara: antes todo era texto, ahora son paquetes binarios compactos.  
- Con ASCII, dependía de leer un string hasta el salto de línea, separar por comas y luego convertir cada pedacito en número.    
- Con binario, simplemente leo 6 bytes y ya sé que allí están todos mis datos en orden.    
Esto hace la comunicación más rápida y eficiente, pero también más exigente: si se me descuadra un byte, ya no hay comas ni saltos de línea que me salven.  

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
--- 
### Rubrica 
| Item | Nota | Justificación |
| --- | --- | --- |
| **1. Profundidad de la Indagación** | 5.0 | Con base en lo trabajado en la bitácora, se evidencia una exploración crítica sobre cómo el micro:bit envía y organiza los datos, qué ventajas y limitaciones tiene el uso de ASCII y cómo se interpretan estos mensajes en p5.js. No solo se describió la estructura del protocolo, también se plantearon escenarios que ponen a prueba su diseño, como el envío de más valores de los previstos, la llegada de datos incompletos o la posibilidad de agregar validaciones extra como checksums. Estas preguntas llamadas dudas que están a lo largo del documento y respuestas muestran una reflexión sobre las implicaciones de la comunicación de datos: claridad frente a eficiencia, simplicidad frente a robustez, y flexibilidad frente a seguridad. En conjunto, las evidencias justifican que se alcanzó un nivel alto en la indagación, ya que se analizaron no solo los aspectos técnicos inmediatos, sino también las posibles consecuencias y decisiones de diseño que surgen en sistemas de transmisión de información. [ejemplo](#cp1)  |
| **2. Calidad de la Experimentación** | 5.0 | En esta bitácora se nota que no solo probé si el sistema funcionaba, sino que busqué entender por qué fallaba. El error con el checksum me obligó a diseñar un “mini experimento”: al inicio todo marcaba error porque estaba sumando también el byte del encabezado, algo que nunca iba a coincidir con lo enviado por el micro:bit. Para comprobarlo, dejé el código tal cual y observé cómo se repetía el fallo una y otra vez, mostrando “checksum incorrecto” en consola. Ese comportamiento constante fue la evidencia que me permitió aislar el problema y luego validar la solución cambiando solo una línea (`packet.slice(1, 7)`). Al corregirlo, los círculos empezaron a moverse y los botones respondían bien, confirmando que el framing y la verificación de datos estaban funcionando como debían. |
| **3. Análisis y Reflexión** | 5.0 | no me quedaste en describir lo que pasa técnicamente, sino que fui más allá y reflexione sobre las ventajas y desventajas de cada forma de transmisión (ASCII vs binario). Mostre que entiendo no solo “qué hace el código”, sino también los compromisos detrás de esas decisiones: legibilidad contra eficiencia, simplicidad contra robustez. Además, construí una visión completa del proceso, desde que el micro:bit empaqueta los datos hasta que p5.js los interpreta, validando y transformando la información. Eso demuestra un modelo mental sólido del flujo de datos y una reflexión crítica que conecta teoría con práctica, justo lo que pide el nivel más alto de la rúbrica. |
| **4. Apropiación y Articulación de Conceptos** | 5.0 | cumple con este criterio porque no me limité a describir los ejercicios, sino que expliqué los conceptos como un sistema interdependiente. Mostré cómo la comunicación serial funciona como un flujo de bytes asíncrono que, en apariencia, puede ser caótico, pero al aplicar un protocolo se organiza para garantizar orden, confiabilidad y eficiencia. Además, utilicé analogías propias que me permitieron articular estas ideas de forma clara, evidenciando que entendí no solo el funcionamiento, sino también la lógica detrás de este proceso. |
| Promedio | 5.0 |  |

