
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

## Actividad 3  
### Experimento 1  
<img width="1037" height="1026" alt="image" src="https://github.com/user-attachments/assets/0951fe27-ca2e-44f6-9973-4e4ea34bcb4b" />
<img width="1033" height="1027" alt="image" src="https://github.com/user-attachments/assets/de8d3ec1-3031-4aa2-af21-9db5f685c28a" />

Esto me muestra que el servidor relaciona cada URL con una respuesta específica. Si cambio la ruta de /page1 a /pagina_uno, la dirección anterior deja de funcionar porque ya no está definida. Solo al escribir la nueva ruta en el navegador (/pagina_uno) el servidor reconoce la petición y responde. En otras palabras, el servidor depende de cómo se configuren esas rutas para saber qué mostrar.  

### Experimento 2
<img width="1912" height="1032" alt="image" src="https://github.com/user-attachments/assets/72a8424e-6aec-476f-8ac7-ea7253856b3d" />  
ID:aqcgib2Zq4C4zZ8yAAAB  
<img width="1918" height="1032" alt="image" src="https://github.com/user-attachments/assets/104b0898-2754-485b-9190-3fda7d3f28b9" />  
El ID es diferente el de la page 2 es  7_Dw-RxtGJCNrB0TAAAJ

Al desconectar la page 1, aparece este mensaje 
<img width="1025" height="813" alt="image" src="https://github.com/user-attachments/assets/00672c0b-8587-47fc-b88c-db59ec6034b6" />
SI coinciden los ID
<img width="730" height="578" alt="image" src="https://github.com/user-attachments/assets/d7fe505a-dc2e-4f27-97b6-aae95334d8dc" />
Este es el mensaje que aparece al cerrar ambas paginas  

### Experimento 3  
<img width="1902" height="1016" alt="image" src="https://github.com/user-attachments/assets/bb0cc9ac-2cdb-484b-b9ac-a4de0c6a22f2" />  
Se está registrando el evento winUpdate uno y dos, que es el que se encarga de enviar la información de los cambios del cliente. Los datos que aparecen (Data:) muestran valores como la posición en x y y, además del ancho y el alto de la ventana. Por ejemplo, se ven números como x: 835, y: 4, width: 8, height: 770. Esto me indica que cada vez que algo cambia en la ventana, el servidor guarda y comparte esos datos para mantener todo sincronizado.  

<img width="1905" height="1012" alt="image" src="https://github.com/user-attachments/assets/89237a90-37ab-4037-ba66-7f90388e48c1" />  
Al mover la ventana de page2, en la terminal se está registrando el evento win2update. Los datos (Data:) que aparecen muestran cómo cambian las coordenadas de la ventana, con valores en x y y, además del ancho y alto. Esto indica que cada vez que la ventana de page2 se mueve, se actualiza su posición en la terminal mostrando la nueva ubicación junto con sus dimensiones.

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/23cc9ac0-fe07-44f0-9b33-7d6419da365d" />  
<img width="1602" height="582" alt="image" src="https://github.com/user-attachments/assets/859a7510-ad10-46d5-a2f2-5b32888d914b" />

Al hacer el cambio de línea la page1 no actualiza creando un daño en esa conexión que habia entre pages. 
Cuando uso "socket.emit", el servidor responde únicamente al mismo cliente que mandó el evento (no comparte la info con los demás), en cambio "socket.broadcast.emit" envía ese mismo dato a todos los demás clientes conectados excepto el que lo originó, lo que permite que todos se mantengan sincronizados entre sí.

### Experimento 4  
<img width="721" height="401" alt="image" src="https://github.com/user-attachments/assets/49096004-7b9d-4c51-bcb4-a8096b331a15" />  
Esta escuchando el puerto 3001, y si abro en mi web el http://localhost:3000 me genera un error
<img width="1032" height="1066" alt="image" src="https://github.com/user-attachments/assets/4b881ee4-8321-420f-80ca-8cf7995318b3" />  
Pero si abro la 3001, si me deja abrir el page 
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/7ef72fd5-9a34-4cb2-b610-f9492ec827b7" />  

#### Aprendizaje   
Aprendí que la variable port define en qué puerto se va a ejecutar el servidor, y la función listen se encarga de poner el servidor a la escucha en ese puerto para poder recibir conexiones y responder a los clientes.  

## Actividad 4  
### Experimento 1  
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/bb4b990d-4848-47fd-b1c6-730f14e171ef" />  
Primero me aparecen en la consola despues de desconectarlo estos errores que significa que como no hay nada escuchando en el 3000, la conexión se rechaza, luego al refrescarlo, aparece esto  
<img width="1022" height="940" alt="image" src="https://github.com/user-attachments/assets/ea3d41ca-253e-4a5e-97a5-370cf6a218a7" />  
Que aunque ya no tenga errores pues ya la pagina no esta disponible pq el servidor no esta escuchando.  
<img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/71270e48-5495-4906-8d56-da086ccb72b8" />  
Desaparecen estos errores y ya la pagina esta esperando su sincronización.  

### Experimento 2   
<img width="1621" height="1078" alt="image" src="https://github.com/user-attachments/assets/45a0f1d8-a112-4bd4-9bed-f856450072e0" />  
<img width="733" height="428" alt="image" src="https://github.com/user-attachments/assets/486bf686-de4a-48e0-a6a8-02f1c7c93b7c" />    

Cuando comente la línea que enviaba los datos de page2 al conectarse, deje de mandar automáticamente su información al servidor al abrir la página. Por eso al inicio no se veía ningún win2update. Sin embargo, cuando movi la ventana de page2, esta empezó a enviar sus actualizaciones de posición y tamaño, y el servidor las recibió, manteniendo a los clientes sincronizados. Básicamente, la sincronización sigue funcionando, pero ya no ocurre automáticamente al conectar, solo cuando hay interacción.  

### Experimento 3  
<img width="1917" height="1006" alt="image" src="https://github.com/user-attachments/assets/3873e39d-472e-4be2-b849-2c940016fcfd" />   
Se movio la pagina 1 y la información de posición se mostro en la pagina 2 y al contrario  
<img width="1918" height="1012" alt="image" src="https://github.com/user-attachments/assets/f3f63a76-34e3-4155-b380-e11c3e1fd012" />  
se mostraron los datos en la pag1  
Esto sucede porque el servidor es el que transmite la información entre las páginas: una página manda los datos y el servidor se encarga de enviárselos a la otra. Por eso los cambios solo se ven reflejados en la consola de la página opuesta, y no en la misma desde donde se hacen.  

### Experimento 4  
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/1eaa3733-46c6-480b-858f-5333f80b6458" />  
Se hizo un cambio en el codigo:  

<img width="1590" height="1078" alt="image" src="https://github.com/user-attachments/assets/dceee662-747f-421b-9f67-f0bd8908db6d" />  

Puedo concluir que el código dentro del if solo se ejecuta cuando la ventana realmente cambia de posición o de tamaño, porque esa condición compara los valores actuales con los anteriores y solo entra si encuentra diferencias. Esto demuestra que el programa no está mandando datos todo el tiempo, sino únicamente cuando hay un cambio real.  

### Expiramento 5  
El cambio que se dui fue este 
<img width="1918" height="982" alt="image" src="https://github.com/user-attachments/assets/76b670a5-4d1e-4f76-aae8-7868825e8ba0" />
Este fue mi pequeño experimento   
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/99a1aa2d-aa7d-46ea-9f06-f82f3a9e7e5b" />
cambia de color cuando esta encima del circulo y tambíen se reduce su tamaño simulando un ojo  
```javascript
function draw() {
    background(220);
    
    if (!isConnected) {
        showStatus('Conectando al servidor...', color(255, 165, 0));
        return;
    }
    
    if (!hasRemoteData) {
        showStatus('Esperando conexión de la otra ventana...', color(255, 165, 0));
        return;
    }
    
    if (!isFullySynced) {
        showStatus('Sincronizando datos...', color(255, 165, 0));
        return;
    }

    // Solo dibujar cuando esté completamente sincronizado
    drawCircle(point2[0], point2[1]);
    checkWindowPosition();
    
    let vector2 = createVector(remotePageData.x, remotePageData.y);
    let vector1 = createVector(currentPageData.x, currentPageData.y);
    let resultingVector = createVector(vector2.x - vector1.x, vector2.y - vector1.y);
    
    // Calculamos la distancia entre los puntos
    let d = dist(point2[0], point2[1], resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2);
    
    stroke(50);
    strokeWeight(20);

    // Cambiamos el color del círculo dependiendo de la distancia
    if (d < 100) { 
        fill(0, 255, 0);  // Verde
    } else {
        fill(255, 0, 0);  // Rojo
    }

    // Dibujar el círculo
    ellipse(resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2, 50, 50);

    // Dibujar la línea
    line(point2[0], point2[1], resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2);
}
````

## Actividad 5  
### Idea 
Respira y brilla 

El círculo tenga un halo suave. A medida que se acerquen, el brillo no solo crece, sino que cambia de color (de azul frío a amarillo brillante).

Pulso rítmico: Los círculos no se quedan estáticos: su tamaño oscila suavemente (como si inhalaran/exhalaran).

- Cuando están cerca, el pulso se acelera un poquito.

- Cuando están lejos, pulsan más despacio.

Luz ambiental: Además del glow, el fondo también responde. El background va de un gris neutro a un degradado de color luminoso cuando la distancia es corta (como si todo el espacio respirara con ellos).

Efecto de “choque de energía”: Si se acercan demasiado (menos de 50px), ocurre un destello rápido (círculos se vuelven blancos y salen estrellas con destellos del centro).

