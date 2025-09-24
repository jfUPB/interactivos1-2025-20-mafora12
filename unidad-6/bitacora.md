
# Evidencias de la unidad 6

## Actividad 1   
### 1. ¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?  
Cuando se ejecuto el comando en git bush se ejecuto la instalación en las dependencias del repositorio   clonado anteriormente. EL mensaje dice que  se añadieron al  repositrio 120 paquetes y verifico 121 en   segundos.  
En conclusión el mensaje dice que la instalación fue exitosa, no hay errores ni problemas de seguridad.

### 2. ¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?   
   ```bash
    > nodejs-test-1@1.0.0 start
    > node server.js

    Server is listening on http://localhost:3000
   ```  
Esta corriendo el script llamado start del proyecto "nodejs-test-1" en su versión 1.0.0. Luego este script ejecuta el archivo server.js usando Node.js y por ultimo indica que el servidor esta recibiendo los datos en la pagina.
Así, tu servidor ya está corriendo y si entro a http://localhost:3000 desde el navegador, puedo ver lo  que tu aplicación devuelve

### 3. Describe lo que ves inicialmente en page1 y page2 en tu navegador.  
Se puede observar un circulo rojo con borde de color gris oscuro en la page1 que lo une una linea al page2 que contiene otro circulo con las mismas caracteristicas.  
<img width="1918" height="1062" alt="image" src="https://github.com/user-attachments/assets/89b3ec23-c17b-4f94-93d1-d8a5a9d8a78f" />

### 4. ¿Qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?  
Al abrir page1 y page2, la terminal mostró la conexión de cada usuario con su ID único, las actualizaciones de datos (posición y tamaño de las ventanas) y confirmaciones de que ambos clientes estaban conectados y completamente sincronizados.

### 5. Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas. ¿Cambia algo visualmente? ¿Qué mensajes aparecen (si los hay) en la consola del navegador (usualmente accesible con F12 -> Pestaña Consola) y en la terminal del servidor?   
Cuanda acerco una de las paginas estas se conectan y la distancia de la linea se acorta y si la pongo una encima de la otra se vuelve un solo circulo si pongo preciso en su centro, si no solo la línea que las une se vuelve corta.  
En la consola primero aparece la conección del cliente, aparece que no esta sincronizado, luego recibe los datos del servidor que son los valores de los circulos posición alto y ancho y finalmente confirma si está o no sincronizado.

## Actividad 2  
### ¿Qué es Internet?  
#### 1. ¿Qué pasaría si esa rampa se corta? Anota tus ideas.   
- No habría acceso a Internet o mi vehículo no podría entrar a la carretera y me quedaría aislada desde el dispositivo que este usando, aunque todo lo demás funcione.  
- Perdería la comunicación en tiempo real con otras personas.  
- solo podria usar apps sin conexión.  

### Navegador y servidor  
#### 2. ¿Puedes identificar otros ejemplos de relaciones Cliente-Servidor en tu vida diaria (no necesariamente digitales)? Por ejemplo, al pedir comida en un restaurante. ¿Quién es el cliente y quién el servidor? ¿Qué se pide y qué se entrega?  
- cajero Automatico:  
     - Cliente: la persona con su tarjeta.  
     - Servidor: el cajero automático conectado al banco.  
     - Se pide: dinero.  
     - Se entrega: la información, respuestas y el monto solicitado.  
- Hotel:  
     - Cliente: el huésped.  
     - Servidor: la recepción/servicio de habitaciones.  
     - Se pide: hospedaje, comida o limpieza.  
     - Se entrega: la habitación, los servicios solicitados.  
- Cine:  
     - Cliente: el espectador.  
     - Servidor: la taquilla o boletería.  
     - Se pide: una entrada para la película.  
     - Se entrega: el tiquete o código de acceso.

### ¿Qué es una URL?  

#### El nombre de dominio y la ruta (si la hay). ¿Qué crees que pasa si solo escribes el nombre de dominio (ej. www.google.com) sin una ruta específica? ¿Qué “página por defecto” crees que te envía el servidor?   

#### Pagina:  
https://loveanddeepspace.infoldgames.com/en-EN/home#0 

- Protocolo:
   "https://": indica que el navegador y el servidor se comunican usando HTTPS.   
- Nombre de dominio:  
   "loveanddeepspace.infoldgames.com": Es el dominio, que apunta al servidor donde está alojada la página del juego.  
- Ruta:  
   "/en-EN/home": Es la ruta dentro del servidor. Aquí le dice que muestre la versión en inglés de la página principal del sitio (home).
  
#### Duda: ¿Qué es #0?  
"#0": es un ancla interna dentro de la página. No lo maneja el servidor, sino el navegador, que usa ese valor para desplazarse a una sección específica del documento.  

- Si le quito la ruta el servidor me redirije automáticamente a la página inicial del sitio (la “home”), en el idioma que esta por defecto, el inglés.
  
### Protocolo HTTP  
#### 1. ¿Qué similitudes encuentras?  
- Ambos son protocolos de comunicación, es decir, conjuntos de reglas que permiten que dos sistemas distintos se entiendan.   
- Tanto en HTTP como en los protocolos seriales (ASCII/binario con framing) hay una estructura clara de mensaje → respuesta.  
- En los dos casos se necesita un formato acordado (cómo empieza y termina el mensaje, qué datos contiene) para que no haya confusión.

#### 2. ¿Qué diferencias clave ves?  
- El protocolo serial es mucho más básico: solo envía bytes o caracteres por un canal, casi como pasar mensajes letra por letra.  
- HTTP en cambio trabaja a una escala mucho mayor: no solo manda datos, también incluye información adicional (códigos de estado, tipo de archivo, cabeceras, etc.).  
- El serial es directo entre dos dispositivos conectados (ej. micro:bit ↔ PC).  
- HTTP funciona en una red global (Internet), donde intervienen muchos más actores (clientes, servidores, routers, dominios).
  
#### 3. ¿Por qué crees que HTTP necesita ser más complejo que un simple envío de bytes como hacías con el micro:bit?   
Porque no basta con mandar bytes sueltos, en la web el navegador necesita saber qué archivo recibió, qué tipo de datos son (HTML, imagen, audio, JSON), si hubo errores (404 Not Found, 500 Error), e incluso manejar temas de seguridad (HTTPS).
HTTP agrega estructura, contexto y control para que la comunicación sea segura y clara a escala mundial.  

### HTML, CSS y JavaScript  
#### ¿Qué parte crees que es HTML (ej. los campos de texto, el botón)?, ¿Qué parte es CSS (ej. el color del botón, el tipo de letra)?, ¿Qué parte es JavaScript (ej. la comprobación de si escribiste algo antes de enviar, el mensaje de “contraseña incorrecta” que aparece sin recargar la página)?  
- HTML: serían los campos donde escribo el usuario y la contraseña, y también el botón para ingresar, o sea la base del formulario.
- CSS: El diseño, osea el color del botón, la letra que se usa o el tamaño de los cuadros de texto.
- JavaScript: es lo que hace que el formulario funcione, como revisar que yo sí haya escrito algo antes de enviar, o mostrar un mensaje de error de “contraseña incorrecta” sin necesidad de recargar la página.

### El Modelo de ejecución: imperativo vs basado en eventos  
#### 1. ¿Qué ventajas crees que tiene el modelo basado en eventos para una interfaz de usuario web?  
El modelo basado en eventos hace que la página sea más eficiente y rápida, porque solo reacciona cuando ocurre algo (clics, mensajes, redimensionar la ventana, etc.). Esto evita gastar recursos innecesarios y hace que la interfaz responda justo en el momento en que el usuario interactúa.   

#### 2. ¿Sería eficiente tener un bucle draw() redibujando toda la página 60 veces por segundo si nada ha cambiado?  
No sería eficiente, porque la página estaría consumiendo memoria y procesador sin necesidad. Redibujar todo constantemente sería un desperdicio de recursos si el contenido sigue igual. En la web es mejor actualizar solo lo que cambia cuando ocurre un evento.  

### ¿Qué es Node.js?  
#### ¿Por qué crees que podría ser útil usar JavaScript tanto en el cliente (navegador) como en el servidor? ¿Se te ocurre alguna ventaja para los desarrolladores?  
Usar JavaScript tanto en el cliente como en el servidor es muy útil porque unifica el desarrollo en un solo lenguaje, lo que facilita la comunicación, la comprensión del proyecto y ahorra tiempo al evitar aprender lenguajes distintos para cada lado. Gracias a esto, los desarrolladores pueden reutilizar código, reducir errores y trabajar de forma más fluida, logrando que la creación y el mantenimiento de las aplicaciones sean más simples y eficientes.  
### WebSockets y Socket.IO    
#### Resume con tus propias palabras la diferencia fundamental entre una comunicación HTTP tradicional y una comunicación usando WebSockets/Socket.IO. ¿En qué tipo de aplicaciones has visto o podrías imaginar que se usa esta comunicación en tiempo real?  
La diferencia principal es que en la comunicación HTTP tradicional el cliente siempre tiene que pedir algo y el servidor responder, como un ida y vuelta por mensaje; en cambio, con WebSockets/Socket.IO se establece una conexión continua en la que ambos pueden enviarse datos de manera instantánea sin esperar una nueva petición. Este tipo de comunicación en tiempo real se usa, por ejemplo, en aplicaciones de chat, videollamadas, juegos en línea, o incluso en plataformas colaborativas como Google Docs, donde varias personas pueden ver cambios al mismo tiempo.  

