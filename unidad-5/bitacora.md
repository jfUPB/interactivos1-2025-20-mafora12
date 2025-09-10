# Evidencias de la unidad 5

## Actividad 2

### ¿Qué datos envía el micro:bit?  
El micro:bit se comunica con el sketch en p5.js a través del puerto serie (UART) usando un protocolo ASCII. Cada 100 ms, el micro:bit toma lecturas del acelerómetro en los ejes X y Y, además del estado de los botones A y B, y construye una cadena con esos valores separada por comas y terminada en un salto de línea:  

``` js
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
uart.write(data)
```
Brinda legibilidad y depuración, ya que al usar ASCII es fácil abrir un monitor serial y ver directamente qué está transmitiendo el micro:bit. Sin embargo, esto se logra a costa de cierta ineficiencia en el uso de bytes, pues cada número es transmitido como caracteres y no como datos binarios compactos.

### ¿Cómo es la estructura del protocolo ASCII usado?
El protocolo sigue una estructura simple:  
``` js
xValue, yValue, aState, bState\n
```

"xValue, yValue" -> enteros del acelerómetro.  

"aState, bState"->booleanos (True o False).  

"," → delimitador que permite dividir los datos.  

"\n" → marca el final de la transmisión.  

Esto es un poco frajil y poco optimizado debido a que si el delimitador se pierde o se altera, toda la lectura queda dañada. En contraste, un protocolo binario sería más robusto en eficiencia, pero menos intuitivo. Aquí se esta priorizando la claridad sobre la optimización.  

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

Aquí se aprecia cómo los datos brutos enviados en ASCII se reinterpretan como coordenadas de pantalla (X, Y) y como estados lógicos de botones. Esto evidencia que el protocolo no solo transmite información, sino que también establece un contrato semántico: cada posición en la cadena tiene un significado preciso.  

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

