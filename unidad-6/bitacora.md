
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

### 1. ¿Qué pasaría si esa rampa se corta? Anota tus ideas.   
- No habría acceso a Internet o mi vehículo no podría entrar a la carretera y me quedaría aislada desde el dispositivo que este usando, aunque todo lo demás funcione.  
- Perdería la comunicación en tiempo real con otras personas.  
- solo podria usar apps sin conexión.

### 2. ¿Puedes identificar otros ejemplos de relaciones Cliente-Servidor en tu vida diaria (no necesariamente digitales)? Por ejemplo, al pedir comida en un restaurante. ¿Quién es el cliente y quién el servidor? ¿Qué se pide y qué se entrega?  
- cajero Automatico
     - Cliente: la persona con su tarjeta.
     - Servidor: el cajero automático conectado al banco.
     - Se pide: dinero
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
