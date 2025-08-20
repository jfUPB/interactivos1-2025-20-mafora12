# Unidad 2


## 🤔 Fase: Reflect

### Actividad 6.

#### Parte 1  

1. Para mí, una máquina de estados es como un mapa de rutas que me dice en qué “modo” o situación está mi programa y qué puede pasar para que cambie a otro modo. En este proyecto usé cuatro componentes principales: los estados (por ejemplo, configuración, armado y explosionado), los eventos (acciones como presionar un botón o agitar el micro:bit), las transiciones (los caminos que llevan de un estado a otro) y las acciones (lo que el sistema hace cuando sucede un evento o cuando está en un estado).  

2. La técnica de máquina de estados me parece súper útil para manejar varias cosas “al mismo tiempo” en el micro:bit, que solo tiene un hilo de ejecución. Gracias a esto puedo atender un temporizador y los botones sin que uno bloquee al otro. Esto evita problemas que tendría si usara funciones como sleep(), ya que con sleep() el programa se quedaría detenido y no respondería hasta que pasara el tiempo, mientras que con la máquina de estados puedo seguir revisando eventos constantemente.

3. Si quisiera agregar la nueva funcionalidad de que, al agitar el micro:bit durante la cuenta regresiva, el tiempo se reduzca a la mitad, en mi diagrama añadiría un nuevo evento “shake” dentro del estado ARMED. La acción asociada sería dividir el tiempo restante por 2, y luego el flujo continuaría en el mismo estado armado hasta que llegue a cero o se reinicie.

4. Para mí, un vector de prueba es como una pequeña receta para comprobar que todo funciona como debería. Describe la situación inicial, lo que hago (evento) y lo que espero que pase. Es importante porque me permite comprobar paso a paso si cada parte de la máquina de estados está funcionando y detectar rápido si algo no va como planeé.

#### Parte 2.
1. En este proyecto, lo que más me costó fue pasar del diagrama de estados al código en MicroPython. Dibujar el diagrama fue más sencillo porque podía visualizarlo todo, pero al pasarlo a código tuve que pensar en cómo hacer que las transiciones y las acciones se cumplieran en el orden correcto y sin bloquear el resto del programa.  

2. Un error que encontré fue que, al inicio, el temporizador no se actualizaba como quería porque el control del tiempo estaba mal calculado con utime.ticks_ms(). Pensar en términos de estados, eventos y transiciones me ayudó a darme cuenta de que ese fallo estaba dentro de la lógica del estado ARMED, y así pude corregirlo ajustando la comparación del tiempo transcurrido.  

3. Mi estrategia para resolver la bomba fue empezar con una versión muy básica: primero solo configuré el tiempo y lo mostré en pantalla, luego agregué el armado y la cuenta regresiva, y por último la explosión y el reinicio. Ir paso a paso me ayudó a no perderme y a detectar errores antes de que se mezclaran con otras funciones.  

4. Ahora que entiendo cómo funciona una máquina de estados, creo que la podría usar en un videojuego sencillo, por ejemplo, un juego de aventuras donde el personaje pueda estar en estados como “explorando”, “combatiendo”, “herido” o “descansando”. Así controlaría las acciones disponibles en cada momento y haría que la experiencia sea más ordenada y fluida.



### Actividad 7.  

En el diseño e implementación de mi compañero encuentro varios puntos positivos. Me parece acertado que haya utilizado "was_pressed()" para la lectura de botones, ya que permite detectar pulsaciones únicas y evita que se registren incrementos o decrementos múltiples al mantener el botón presionado. También destaco que en el conteo del tiempo haya utilizado la condición "<= 0" para detonar, ya que es más robusta y segura ante posibles desajustes en la temporización. La estructura del código es clara y sencilla de seguir, y la inclusión de la opción para volver al estado de configuración desde el estado armado con el toque del logo es una buena decisión de usabilidad.

Sin embargo, veo oportunidades de mejora. La animación de explosión está implementada de forma bloqueante con un bucle "for" y pausas "(sleep)", lo que impide que el sistema detecte interacciones (como el toque para reinicio) hasta que termina la animación. Esto podría mejorarse haciendo la animación no bloqueante o verificando la entrada del usuario durante la secuencia, para permitir reiniciar en cualquier momento. También recomendaría estandarizar el uso de nombres de variables para mayor coherencia y utilizar una función dedicada a mostrar el tiempo en pantalla, evitando el uso prolongado de "display.scroll", ya que este puede hacer que el sistema no responda a eventos mientras se ejecuta. Finalmente, sería útil actualizar visualmente el valor de tiempo de forma inmediata cuando se presionan los botones, para que el usuario tenga retroalimentación instantánea.

En resumen, el código de mi compañero es funcional y claro, pero podría optimizarse para mejorar la experiencia de usuario y la respuesta del sistema ante eventos.

### Actividad 8.

- Continuar:
La actividad que más me ayudó a entender el poder de las máquinas de estados fue cuando analizamos ejemplos prácticos y sencillos antes de pasar a un proyecto más complejo. Ver un caso real paso a paso me permitió entender cómo los eventos y las transiciones se relacionan. Creo que el elemento indispensable que se debería mantener es la combinación de diagramas visuales con el código, porque esa conexión entre lo que imagino y lo que programo fue clave para comprenderlo todo.

- Dejar de hacer:
Hubo momentos en los que las instrucciones parecían saltar entre conceptos sin cerrar del todo el anterior. Por ejemplo, en algunas partes pasábamos de la teoría directamente a un ejercicio complejo sin repasar lo básico. Yo eliminaría o reduciría esos saltos grandes y haría una transición más suave para que no se sienta como un cambio brusco.

- Empezar a hacer:
Me habría ayudado tener más ejemplos intermedios entre el más básico y el proyecto final. Algo así como ejercicios “puente” para practicar cada componente de la máquina de estados de forma aislada antes de unirlo todo.

- Ritmo y dificultad:
Le pondría un 4. No fue imposible, pero sí representó un salto grande pasar del análisis de un programa ya hecho (Actividad 03) a diseñar desde cero uno más complejo (Actividad 04 y 05). La dificultad estuvo en que, cuando el programa crecía, había que pensar en varios eventos y transiciones al mismo tiempo, y eso requería mucha organización mental.

- Comentario adicional:
Hubo momentos de frustración cuando el programa no reaccionaba a tiempo o se quedaba “congelado” por errores en el manejo del tiempo. Pero también tuve un momento “¡Aha!” cuando entendí que no se trataba de hacer todo en un solo bloque, sino de dejar que cada estado controlara su parte y que los eventos fueran la llave para cambiar de uno a otro. Ese momento me hizo sentir que ya dominaba la lógica de las máquinas de estados.

  


