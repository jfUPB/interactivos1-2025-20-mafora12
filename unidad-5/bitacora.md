# Evidencias de la unidad 5

## ¿Qué datos envía el micro:bit?  
El micro:bit se comunica con el sketch en p5.js a través del puerto serie (UART) usando un protocolo ASCII. Cada 100 ms, el micro:bit toma lecturas del acelerómetro en los ejes X y Y, además del estado de los botones A y B, y construye una cadena con esos valores separada por comas y terminada en un salto de línea:
js ````
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
uart.write(data)
