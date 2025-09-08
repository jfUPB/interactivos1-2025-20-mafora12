# Unidad 2
## 🛠 Fase: Apply
### Actividad 4
#### Diseño de la lógica de una bomba temporizada
![Diagrama de la maquina de estado](https://www.canva.com/design/DAGvU67JuO8/_dCyHHNlJYEWAHGjNKvB0w/edit?utm_content=DAGvU67JuO8&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

### Actividad 5
#### Implementando la Bomba Temporizada
##### Codigo
```python
from microbit import *
import utime

State_Init = 0
State_Settings = 1
State_Armed = 2
State_Exploted = 3

interval = 0
current_State = State_Init
cero = 0

def bomba():
    global interval
    global current_State
    global cero
    if current_State==State_Init:
        interval=20
        display.show(interval)
        display.clear()
        current_State=State_Settings
        
    elif current_State == State_Settings:
        
        if button_a.was_pressed():
            if interval<60:
                interval = interval + 1
                display.show(interval)
                display.clear()
                current_State=State_Settings
                
        if button_b.was_pressed():
            if interval >10:
                interval = interval - 1
                display.show(interval)
                display.clear()
                current_State=State_Settings

        if accelerometer.was_gesture('shake'):
            current_State=State_Armed
            
    elif current_State == State_Armed:
    
        for i in range (interval,cero -1,-1):
            display.show(str(i))
            utime.sleep(1)
        
        display.scroll('BUM')
        current_State = State_Exploted

    elif current_State == State_Exploted:
        display.show(Image.SKULL)
        if pin_logo.is_touched():
            current_State=State_Init

    else:
        pass

while True:
    bomba()
```
##### Vectores de prueba
1. Si estoy en el estado Settings, presiono el boton A, deberia sumarle un segundo al tiempo incial y volver a estado Settings para esperar otra "indicacion". (Funciona)
2. Si estoy en el estado Settings, presiono el boton B, deberia restarle un segundo al tiempo incial y volver a estado Settings para esperar otra "indicacion". (Funciona)
3. Si estoy en el estado Settings, mando la indicacion de 'shake', deberia cambiar de estado. (Funciona)
4. Si estoy en el estado Armed, el primer numero que deberia ver es el tiempo que modifique o no en el estado anterior, y ver la cuenta regresiva hasta 0. (Funciona)
5. Si estoy en el estado Armed, una vez la cuenta regresiva termine, es decir, llega a cero, debe mostrar un texto que diga 'BUM' y debe cambiar de estado. (Funciona)
6. Si estoy en el estado Exploted, debe mostrar una calavera, y una vez se presione el 'touch' debe inicializar el tiempo al original, y cambiar de estado. (Funciona)
7. Si cambie de estado Exploted a estado Settings, al presionar A o B, debe cambiar el tiempo inicial. (Funciona)



## 🛠 Fase: Apply


### Actividad 4.  
![Diagrama de estados](https://github.com/user-attachments/assets/56efb5f4-cc53-4f4a-a63d-79a91c50e4cb)  

### Actividad 5.  

#### 1. 
```javascript
from microbit import *
import music
import utime

# ------------------------------
# Estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLODED = 2

# ------------------------------
# Variables globales
current_state = STATE_CONFIG
timer_value = 20  # Tiempo inicial en segundos
min_time = 10
max_time = 60
last_time = utime.ticks_ms()
countdown_time = 0 
# ------------------------------
# Funciones auxiliares

def show_time(t):
    display.show(str(t)[-1]) if t < 10 else display.scroll(str(t))

def explode():
    display.show(Image.SKULL)
    music.play(music.WAWAWAWAA)

# ------------------------------
# Bucle principal
while True:
    if current_state == STATE_CONFIG:
         # Modo configuración
        show_time(timer_value)
        
        # Aumentar tiempo
        if button_a.is_pressed():
            timer_value += 1
            if timer_value > max_time:
                timer_value = max_time
            show_time(timer_value)
            sleep(300)

        # Disminuir tiempo
        if button_b.is_pressed():
            timer_value -= 1
            if timer_value < min_time:
                timer_value = min_time
            show_time(timer_value)
            sleep(300)

        # Armar bomba con gesto shake
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            countdown_time = timer_value
            last_time = utime.ticks_ms()
            display.clear()

    elif current_state == STATE_ARMED:
        current_time = utime.ticks_ms()
        if utime.ticks_diff(current_time, last_time) >= 1000:
            countdown_time -= 1
            last_time = current_time
            if countdown_time >= 0:
                show_time(countdown_time)
        
        # Si llega a cero, explota
        if countdown_time == 0:
            explode()
            current_state = STATE_EXPLODED

    elif current_state == STATE_EXPLODED:
        # Esperar toque para reiniciar
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            timer_value = 20
            display.clear()
```

#### 2. 

- Vector de Prueba 1 – Aumentar el tiempo con botón A  
    Estado Inicial: STATE_CONFIG  
    Evento: Presionar el botón A  
    Resultado esperado: El valor del temporizador aumenta en 1 unidad  
    Acción esperada: El valor mostrado en pantalla se actualiza; si se supera el valor máximo (60), se mantiene en 60  
    Resultado obtenido: El sistema aumenta correctamente el tiempo en 1 segundo por cada pulsación. Al llegar a 60, no permite incrementos mayores. La pantalla muestra el valor actualizado.
    
- Vector de Prueba 2 – Disminuir el tiempo con botón B  
    Estado Inicial: STATE_CONFIG  
    Evento: Presionar el botón B  
    Resultado esperado: El valor del temporizador disminuye en 1 unidad  
    Acción esperada: El valor mostrado en pantalla se actualiza; si se baja del mínimo (10), se mantiene en 10  
    Resultado obtenido: El sistema disminuye el valor del temporizador correctamente. Si se intenta bajar de 10, se mantiene en ese valor sin errores.
  
-  Vector de Prueba 3 – Activar la bomba con el gesto shake  
    Estado Inicial: STATE_CONFIG  
    Evento: Realizar el gesto “shake” con el micro:bit   
    Resultado esperado: Cambio al estado STATE_ARMED  
    Acción esperada: Se limpia la pantalla y se inicia la cuenta regresiva desde el valor configurado
    Resultado obtenido: Al agitar el micro:bit, el sistema cambia correctamente al estado armado, limpia el display e inicia la cuenta regresiva desde el tiempo seleccionado    

- Vector de Prueba 4 – Cuenta regresiva en estado armado  
    Estado Inicial: STATE_ARMED  
    Evento: Paso de 1 segundo (tick de reloj)  
    Resultado esperado: Decremento de countdown_time en 1 unidad cada segundo  
    Acción esperada: Se muestra el nuevo valor en pantalla correctamente, reflejando el paso del tiempo
    Resultado obtenido: El conteo disminuye de 1 en 1 cada segundo. La pantalla muestra correctamente los valores intermedios del temporizador.   

- Vector de Prueba 5 – Explosión al llegar a cero  
    Estado Inicial: STATE_ARMED  
    Evento: countdown_time llega a 0  
    Resultado esperado: Ejecución de la función explode()  
    Acción esperada: Se muestra una calavera en pantalla y suena un tono de alerta; el estado cambia a STATE_EXPLODED
    Resultado obtenido: Al llegar a 0, el sistema muestra la imagen de calavera y reproduce el sonido de explosión. El estado se actualiza correctamente a STATE_EXPLODED.   
  
- Vector de Prueba 6 – Reinicio tras explosión  
    Estado Inicial: STATE_EXPLODED  
    Evento: Tocar el logo (pin_logo)  
    Resultado esperado: Reinicio del sistema  
    Acción esperada: Se vuelve al estado STATE_CONFIG, se restablece el temporizador a 20 y se limpia la pantalla
    Resultado obtenido: Al tocar el logo, el sistema reinicia con éxito: vuelve al estado de configuración, el temporizador se establece en 20, y se limpia el display.

- Vector de Prueba 7 – Permanencia en Config sin interacción  
    Estado Inicial: STATE_CONFIG  
    Evento: No hay interacción del usuario (ni botones ni gestos)  
    Resultado esperado: El valor del temporizador se mantiene sin cambios  
    Acción esperada: Se sigue mostrando el mismo valor en pantalla sin ejecutar transiciones de estado
    Resultado obtenido: El sistema permanece en estado de configuración sin variaciones. El temporizador se mantiene estable mientras no hay entrada del usuario.

