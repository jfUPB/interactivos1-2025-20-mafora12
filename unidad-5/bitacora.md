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
<img width="1230" height="541" alt="image" src="https://github.com/user-attachments/assets/468cc386-595e-476b-ac0d-fa3fd4d534a9" />





