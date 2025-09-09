# Unidad 2

## 🔎 Fase: Set + Seek  

### Actividad 1.  

#### 1.  
Este programa fue desarrollado para la placa micro:bit y utiliza una máquina de estados para manejar el parpadeo de dos píxeles en la pantalla LED. A través de una clase llamada Pixel, se crean dos objetos que representan píxeles ubicados en posiciones diferentes: uno en la esquina superior izquierda (0,0) y otro en la esquina inferior derecha (4,4). Cada uno parpadea a una velocidad distinta: uno cada 1000 milisegundos (1 segundo) y el otro cada 500 milisegundos (0.5 segundos).
El programa usa una lógica de estados para controlar el tiempo y el comportamiento visual de cada píxel. La idea es que cada píxel alterne entre encendido y apagado después de cierto intervalo de tiempo.

#### 2. Estados del programa.  

El objeto "Pixel", creado dentro del programa, funciona como una pequeña máquina de estados. Cada objeto tiene dos estados principales:  

- "Init": Es el estado inicial, donde se configura el píxel por primera vez. Aquí se enciende el píxel y se guarda el tiempo actual.
  
- "WaitTimeout": Es el estado en el que el programa espera que pase un intervalo de tiempo específico. Si ese tiempo ya pasó, el píxel cambia de estado (se apaga si estaba encendido, o se enciende si estaba apagado).

Cada pixel tiene sus propios estados de forma independiente.  

#### 3. Los eventos del programa:   
Los eventos que determinan cuándo cambiar de estado o realizar una acción son:  

- Inicio del programa: cuando se crea el objeto Pixel, este empieza en el estado "Init".

- Paso del tiempo: se utiliza la función utime.ticks_diff(...) > self.interval para verificar si ya pasó el tiempo necesario desde la última acción. Si el tiempo ha pasado, se ejecuta la acción de cambio de brillo.  

#### 3. Acciones en el programa  
Dependiendo del estado y del evento, se ejecutan distintas acciones:  
- En el estado "Init": Se guarda el tiempo actual con "utime.ticks_ms()" y se actualiza el estado a "WaitTimeout" tambvien, se enciende el píxel con el valor inicial (en este caso, 0 que significa apagado).  
- En el estado "WaitTimeout": Si ha pasado el intervalo de tiempo definido, se alterna el valor del brillo del píxel:  
  - Si está en 9 (encendido), se cambia a 0 (apagado).  
  - Si está en 0, se cambia a 9.  
- Se actualiza visualmente el píxel usando display.set_pixel(...).  
- Se actualiza el tiempo de referencia para comenzar a contar nuevamente el intervalo.  


### Actividad 2.
Codigo semaforo:

```javascript
from microbit import *
import time

# Definir estados
RED = 0
YELLOW = 1
GREEN = 2

class TrafficLight:
    def __init__(self):
        self.state = RED
        self.last_change = running_time()  # tiempo del último cambio
    
    def update(self):
        current_time = running_time()
        
        if self.state == RED:
            self.show_red()
            if current_time - self.last_change >= 2000:
                self.state = GREEN
                self.last_change = current_time
        
        elif self.state == GREEN:
            self.show_green()
            if current_time - self.last_change >= 2000:
                self.state = YELLOW
                self.last_change = current_time
        
        elif self.state == YELLOW:
            self.show_yellow()
            if current_time - self.last_change >= 1000:
                self.state = RED
                self.last_change = current_time

    def show_red(self):
        display.clear()
        display.set_pixel(2, 0, 9)  # LED superior para rojo

    def show_yellow(self):
        display.clear()
        display.set_pixel(2, 2, 9)  # LED central para amarillo

    def show_green(self):
        display.clear()
        display.set_pixel(2, 4, 9)  # LED inferior para verde

        # Crear el objeto semáforo
light = TrafficLight()

# Bucle principal
while True:
    light.update()
    sleep(100)  # para que no colapse la CPU
```

- Estados:  
  El semáforo tiene 3 estados principales, que representan el color actual encendido:  
  RED: el semáforo muestra la luz roja.  
  GREEN: el semáforo muestra la luz verde.  
  YELLOW: el semáforo muestra la luz amarilla.  

- Eventos:  
  Los eventos están relacionados con el paso del tiempo:  
  Si el semáforo está en RED y han pasado 2 segundos, cambia a GREEN.  
  Si está en GREEN y han pasado 2 segundos, cambia a YELLOW.  
  Si está en YELLOW y ha pasado 1 segundo, cambia a RED.

- Acciónes:  
  Cada vez que se entra a un estado, se ejecuta una acción específica:  
  En RED: se limpia la pantalla y se enciende el LED superior (posición 2,0).  
  En GREEN: se limpia la pantalla y se enciende el LED inferior (posición 2,4).  
  En YELLOW: se limpia la pantalla y se enciende el LED central (posición 2,2).  

- Resumen del programa:  
  El sistema inicia en el estado RED.    
  Cada cierto tiempo, el estado cambia de acuerdo al temporizador interno.  
  El ciclo de estados es el siguiente:  
  🔴 RED → 🟢 GREEN → 🟡 YELLOW → 🔴 RED … (se repite infinitamente).  
  Entre cada cambio de estado, el micro:bit espera el tiempo correspondiente antes de pasar al siguiente.

- Lógica:
  Se utiliza una clase llamada "TrafficLight" que guarda el estado actual y el tiempo del último cambio.  
  En el método "update()" se verifica si ha pasado suficiente tiempo desde el último cambio de color.  
  Si se cumple el tiempo, se cambia de estado y se actualiza la pantalla con el LED correspondiente.  

### Actividad 3.

#### 1. ¿Por qué decimos que este programa permite realizar de manera concurrente varias tareas?  

Decimos que este programa permite realizar varias tareas de manera concurrente porque no se queda esperando de forma pasiva a que el usuario presione un botón o a que pase un intervalo de tiempo. En cada ciclo del "while True", el sistema:  

- Verifica si se ha presionado el botón A, y al mismo tiempo.  
- Verifica si ya pasó el intervalo de tiempo para cambiar de imagen.  

Estas dos verificaciones ocurren al mismo tiempo, sin que una bloquee a la otra. Así, la micro:bit puede responder a eventos externos (como un botón presionado) y también realizar cambios de estado basados en el tiempo, todo en un solo flujo continuo.

Esto simula un comportamiento multitarea o concurrente, aunque técnicamente solo haya un hilo de ejecución.

#### 2. Estados, eventos y acciones en el programa

1. Estados:  
   STATE_INIT → Estado inicial (pseudoestado, no se repite)  
   STATE_HAPPY → Se muestra la imagen HAPPY   
   STATE_SMILE → Se muestra la imagen SMILE  
   STATE_SAD → Se muestra la imagen SAD  
   
3. Eventos  
   - Presión del botón A (button_a.was_pressed())  
   - Paso del tiempo (utime.ticks_diff(...) > intervalo)
     
4. Acciones  
Dependiendo del estado y del evento, se realizan las siguientes acciones:

- En STATE_INIT:
  Mostrar imagen HAPPY  
  Guardar tiempo actual  
  Cambiar a STATE_HAPPY  
  Establecer intervalo de 1500 ms  

- En STATE_HAPPY:   
  Si se presiona botón A → mostrar SAD, cambiar a STATE_SAD, nuevo tiempo e intervalo 2000 ms  
  Si pasa el tiempo → mostrar SMILE, cambiar a STATE_SMILE, intervalo 1000 ms  

- En STATE_SMILE:  
  Si se presiona botón A → mostrar HAPPY, cambiar a STATE_HAPPY, intervalo 1500 ms  
  Si pasa el tiempo → mostrar SAD, cambiar a STATE_SAD, intervalo 2000 ms   

- En STATE_SAD:
  Si se presiona botón A → mostrar SMILE, cambiar a STATE_SMILE, intervalo 1000 ms   
  Si pasa el tiempo → mostrar HAPPY, cambiar a STATE_HAPPY, intervalo 1500 ms

#### 3. Vectores de prueba:
- Vector de prueba 1
  Condición inicial: el sistema está en STATE_HAPPY  
  Evento generado: presiono el botón A.  
  Resultado esperado:  
    - Imagen cambia a SAD.   
    - Estado cambia a STATE_SAD  
    - Intervalo queda en 2000 ms
  Resultado obtenido: El sistema muestra SAD, actualiza el estado y el tiempo → prueba superada
- Vector de prueba 2
  Condición inicial: el sistema está en STATE_SMILE
  Evento generado: pasa 1 segundo (tiempo > intervalo)
  Resultado esperado:  
    - Imagen cambia a SAD  
    - Estado cambia a STATE_SAD  
    - Intervalo cambia a 2000 ms  
  Resultado obtenido: Al cumplirse el tiempo, muestra SAD y cambia correctamente el estado → prueba superada

- Vector de prueba 3:
  Condición inicial: el sistema está en STATE_SAD
  Evento generado: presiono botón A
  Resultado esperado:
    - Imagen cambia a SMILE
    - Estado cambia a STATE_SMILE
    - Intervalo cambia a 1000 ms
  Resultado obtenido: Se actualiza todo correctamente según lo esperado → prueba superada







  








