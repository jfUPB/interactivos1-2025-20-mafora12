# Unidad 3


## 🤔 Fase: Reflect

### Actividad 8.  

- ¿Qué es una máquina de estados y sus cuatro componentes?  
Una máquina de estados es como un esquema que organiza el comportamiento de un programa en diferentes etapas o “modos” de funcionamiento. Cada estado tiene unas reglas y solo responde a ciertos eventos. Los cuatro componentes que he usado son:  

  - Estados: Las diferentes situaciones o modos (por ejemplo, configuración, armado, cuenta regresiva).  

  - Eventos: Las cosas que pasan y que hacen que el programa reaccione (un botón, un tiempo que se acaba, un dato recibido).  

  - Transiciones: El paso de un estado a otro dependiendo del evento.  

  - Acciones: Lo que el sistema hace cuando se da un evento o cuando entra a un estado (como mostrar algo en pantalla, activar o detener un contador).  

- ¿Por qué es útil para manejar concurrencia?  
Sirve porque, aunque el micro:bit o p5.js solo ejecutan una cosa a la vez, la máquina de estados hace que parezca que está atendiendo muchas cosas al mismo tiempo. En lugar de “pausar” el programa con algo como sleep(), que bloquea todo, el flujo sigue revisando constantemente qué eventos pasan y cambia de estado cuando toca. Esto evita que el programa se quede “congelado” esperando y permite que reaccione rápido a varias cosas.  

- ¿Cómo agregaría la funcionalidad de reducir el tiempo a la mitad?  
En el diagrama, yo añadiría un nuevo evento que solo se escucha cuando el estado es “cuenta regresiva”. Ese evento podría llamarse algo como “evento especial”. Si se detecta, haría que la transición dentro de ese mismo estado ejecute una acción: dividir el tiempo actual entre dos. No sería un estado nuevo, sino una transición extra dentro del mismo estado de cuenta regresiva.  

- ¿Qué es un vector de prueba y por qué es importante?  
Un vector de prueba es como una lista de situaciones que voy a probar para asegurarme de que la máquina funciona bien. Incluye qué entradas se dan, en qué estado estoy y qué espero que pase. Es importante porque permite encontrar errores antes de que se usen en el programa real. Es como un checklist para no dejar nada sin verificar.


### Actividad 9.  
- ¿Qué elemento debería mantener?
Lo que más me ayudó fueron los ejemplos prácticos, como el de la bomba o el temporizador. Ver cómo cada estado iba cambiando y reaccionando a eventos me hizo entender que no es solo teoría, sino que realmente organiza mucho el programa. Creo que lo más importante es mantener los diagramas visuales, porque me ayudan a ver todo el sistema de un vistazo.

- ¿Hubo algún paso o actividad que te pareció confuso? ¿Qué cambiarías o eliminarías?  
Al principio, algunas explicaciones eran muy técnicas, y eso me confundía. A veces se sentía mucha información de golpe. Cambiaría eso por explicaciones más cortas y ejemplos más claros desde el principio, para no sentirse tan abrumada.

- ¿Qué te habría ayudado a entender mejor?
Me hubiera servido tener más ejemplos con cosas cotidianas, como un semáforo o un videojuego sencillo, antes de pasar a algo más complejo como la bomba. También más ejercicios pequeños para practicar antes del proyecto grande.

- ¿Qué tan difícil fue pasar del análisis a diseñar y programar? (1 a 5)  
Yo lo pondría en un 4, porque no es imposible, pero sí requiere mucha atención. Pasar de entender un diagrama a convertirlo en código fue el paso más retador; hay que pensar en detalles que en el papel no se ven, como variables o eventos exactos.

- Comentario adicional:  
Algo que quiero resaltar es que hubo momentos de frustración cuando no me funcionaba algo que parecía lógico, pero también tuve esos momentos de “¡Aha!” cuando encontré el error. Ver el proyecto funcionando fue muy satisfactorio, porque entendí que pensar en estados es como armar un rompecabezas: todo encaja si lo haces paso a paso.

