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



