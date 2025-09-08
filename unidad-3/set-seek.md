# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 5  

#### 1.  

Imagen del modelo de la bomba 3.0:  

<img width="824" height="868" alt="STATE_INIT (1)" src="https://github.com/user-attachments/assets/7b05ac32-5d3a-40b4-92c8-221d2756f95e" />


#### 2.  

| Estado inicial | Evento disparador                                                         | Acciones                                                                    | Estado final           |
| -------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------- | ---------------------- |
| **CONFIG**     | `event.read() == 'A'` (Botón A o UART 'A')                                | `count = min(count+1, 60)` → incrementa el contador y se muestra en display | CONFIG                 |
| **CONFIG**     | `event.read() == 'B'` (Botón B o UART 'B')                                | `count = max(10, count-1)` → decrementa el contador y se muestra en display | CONFIG                 |
| **CONFIG**     | `event.read() == 'S'` (Agitar o UART 'S')                                 | `startTime = utime.ticks_ms()`, limpiar evento, pasar a armado              | **ARMED**              |
| **ARMED**      | Cada 1000 ms (`ticks_diff > 1000`)                                        | `count = count-1`, mostrar en pantalla                                      | ARMED (si `count > 0`) |
| **ARMED**      | `count == 0`                                                              | `display.show(Image.SKULL)`, cambiar estado                                 | **EXPLODED**           |
| **ARMED**      | `event.read() == 'A'`                                                     | Guardar `'A'` en `key[self.keyindex]`, `keyindex += 1`                      | ARMED                  |
| **ARMED**      | `event.read() == 'B'`                                                     | Guardar `'B'` en `key[self.keyindex]`, `keyindex += 1`                      | ARMED                  |
| **ARMED**      | `self.keyindex == len(self.key)` y clave correcta (`passIsOK == True`)    | `count = 20`, mostrar en pantalla, `keyindex = 0`, `state = 'CONFIG'`       | **CONFIG**             |
| **ARMED**      | `self.keyindex == len(self.key)` y clave incorrecta (`passIsOK == False`) | `keyindex = 0` (reinicio de clave)                                          | ARMED                  |
| **EXPLODED**   | `event.read() == 'T'` (logo tocado o UART 'T')                            | Reset: `count = 20`, `display.show(count)`, `startTime = ticks_ms()`        | **CONFIG**             |

