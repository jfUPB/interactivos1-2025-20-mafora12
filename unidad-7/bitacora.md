
# Evidencias de la unidad 7  

## Actividad 1.   
<a name="actividad 1"></a>

<img width="1703" height="1047" alt="image" src="https://github.com/user-attachments/assets/2867dc36-b7ba-4708-8938-ec7809698386" />

### ¿Qué URL de Dev Tunnels obtuviste? ¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte?
<a name="p1"></a>
La URL fue: https://m31xdlbq-3000.use2.devtunnels.ms/
Necesito usar esta URL en lugar de http://localhost:3000 o la IP local del computador porque el celular no está en la misma red local y, por tanto, no puede acceder directamente al servidor que corre en mi equipo.
Dev Tunnels crea un enlace público y seguro en Internet que redirige las peticiones desde esa URL hacia mi servidor local, permitiendo que el celular (u otros dispositivos externos) puedan conectarse y comunicarse con él en tiempo real.

### Describe brevemente qué hace npm install y npm start.
<a name="p2"></a>
  - npm install: Descarga e instala todas las dependencias que el proyecto necesita (las que están listadas en el archivo package.json), para que el código pueda ejecutarse correctamente.
  - npm start: Ejecuta el comando definido en el package.json para iniciar la aplicación —en este caso, levanta el servidor Node.js que usaremos para conectar el celular y el computador.

### ¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?
<a name="p3"></a>
<img width="855" height="320" alt="image" src="https://github.com/user-attachments/assets/a49c6790-6b20-413e-9795-dd8c54b339b3" />
Los mensajes eran iguales para ambos clientes (escritorio y móvil), ya que el servidor solo muestra la conexión o desconexión sin distinguir qué tipo de cliente es. Sin embargo, internamente cada cliente tiene un identificador de conexión diferente (un socket ID) que el servidor usa para reconocerlos individualmente.

### Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?  
<a name="p4"></a>
La interacción era que en el telefono al tocar la pantalla en el espacio gris de la pagina se movia el circulo de la web en el computador. No se me concto de manera lenta, de hecho fue fluido.

## Actividad 2
<a name="actividad 2"></a>
###  Explica con tus propias palabras: ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?  
<a name="p5"></a>
Dev Tunnels es necesario porque mi servidor está corriendo solo en mi computador (localhost), y mi celular no puede entrar ahí directamente. Lo que hace Dev Tunnels es crear una dirección pública que funciona como un puente entre Internet y mi servidor local. Así, cuando abro esa URL desde el celular, Dev Tunnels envía la información hasta mi computador y la devuelve de nuevo, permitiendo que ambos se conecten sin estar en la misma red.  

###  Describe la función de touchMoved() y por qué se usa la variable threshold en el cliente móvil.
<a name="p6"></a>
- La función touchMoved() se ejecuta cada vez que muevo el dedo sobre la pantalla del celular. Sirve para capturar las coordenadas del toque (posición X y Y) y enviarlas al servidor para que el círculo en la pantalla del computador se mueva al mismo tiempo.  
- La variable threshold se usa para evitar que se envíen demasiados datos cuando el dedo apenas se mueve o tiembla un poco. Solo se mandan las coordenadas cuando el movimiento es suficientemente grande, haciendo que la conexión sea más estable y no se sature con mensajes innecesarios.

###  Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?
<a name="p7"></a>
Usar la IP local solo funciona si el celular y el computador están conectados a la misma red Wi-Fi y no hay bloqueos o firewalls. Es rápido y no depende de Internet, pero es limitado porque no sirve si los dispositivos están en redes diferentes.  
En cambio, Dev Tunnels crea una dirección pública accesible desde cualquier lugar, lo que permite conectar el celular aunque esté en otra red o usando datos móviles. Su ventaja es que es más flexible y segura, pero depende de la conexión a Internet y puede ser un poco más lenta que usar la IP local.

### Capturas de pantalla:  
<a name="p8"></a>
#### computador:  
<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/72e5ed00-a9c4-488c-a547-102bee2b8d53" />  

#### celular:  
![Imagen de WhatsApp 2025-10-08 a las 15 09 16_89d00e82](https://github.com/user-attachments/assets/8f60d666-4391-4225-8a42-80e77c518f88)  

#### terminal:
  <img width="1138" height="728" alt="image" src="https://github.com/user-attachments/assets/5a90ede0-190f-4185-bbb0-2ffcddfc6a53" />   

## Actividad 3   
<a name="actividad 3"></a>
### ¿Cuál es la función principal de express.static(‘public’) en este servidor? ¿Cómo se compara con el uso de app.get(‘/ruta’, …) del servidor de la Unidad 6? 
<a name="p9"></a>
La función express.static('public') sirve para que el servidor muestre directamente todos los archivos que están dentro de la carpeta public, sin tener que crear rutas manuales. Así, si entro a /desktop o /mobile, el servidor ya sabe dónde buscar esos archivos.  
En cambio, con app.get('/ruta', …) (como en la Unidad 6) había que escribir una ruta específica para cada página o archivo que queríamos mostrar. Entonces, usar express.static() es más rápido y automático, porque hace todo eso por nosotros.  

### ¿Qué evento lo envía desde el móvil? ¿Qué evento lo recibe el servidor? ¿Qué hace el servidor con él? ¿Qué evento lo envía el servidor al escritorio? ¿Por qué se usa socket.broadcast.emit en lugar de io.emit o socket.emit en este caso?  
<a name="p10"></a>
El mensaje empieza cuando en el celular se activa el evento touchMoved(), que se ejecuta al mover el dedo por la pantalla. Ese evento captura las coordenadas del toque y las envía al servidor con socket.emit('message', datos).  
- En el servidor, ese mensaje se recibe con socket.on('message', ...) y luego el servidor lo reenvía al otro cliente (el del escritorio) usando socket.broadcast.emit('message', message).
- El escritorio escucha ese evento y actualiza la posición del círculo en el canvas.
- Se usa socket.broadcast.emit porque queremos que el servidor envíe el mensaje a todos los demás clientes excepto al que lo mandó. Si usáramos socket.emit, solo se enviaría al mismo cliente, y si usáramos io.emit, se enviaría a todos (incluyendo el celular), lo cual haría que el móvil reciba su propio mensaje innecesariamente.

### Si conectaras dos computadores de escritorio y un móvil a este servidor, y movieras el dedo en el móvil, ¿Quién recibiría el mensaje retransmitido por el servidor? ¿Por qué?  
<a name="p11"></a>
Si conecto dos computadores de escritorio y un celular al servidor, cuando mueva el dedo en el celular, los dos computadores recibirían el mensaje, pero el celular no.  
Esto pasa porque el servidor usa socket.broadcast.emit, que envía el mensaje a todos los demás clientes conectados menos al que lo envió. Entonces, como el celular fue el que mandó la información, el servidor solo la retransmite a los otros clientes (en este caso, los dos computadores).   

### ¿Qué información útil te proporcionan los mensajes console.log en el servidor durante la ejecución?  
<a name="p12"></a>
Los mensajes de console.log me ayudan a ver lo que está pasando en el servidor en tiempo real. Por ejemplo, me muestran cuándo un cliente se conecta o se desconecta, y también los datos que el servidor recibe del celular (como las coordenadas del toque). Gracias a eso puedo saber si la comunicación está funcionando bien o si algo falló durante la conexión.

## Actividad 4   
<a name="actividad 4"></a>
Profe como no entendi muy bien como hacerlo bien completo hice tres varaciones:  
### Variación 1:  

<img width="1697" height="1210" alt="image" src="https://github.com/user-attachments/assets/f72a9d41-5abb-45ca-80fb-5e16ceb237a5" />  

### Variación 2:  

<img width="462" height="1215" alt="image" src="https://github.com/user-attachments/assets/9330c5b1-6a63-484c-a1ec-c78327995aa6" />  

### Variación 3:  

<img width="1598" height="1156" alt="image" src="https://github.com/user-attachments/assets/ba0fb18a-d46c-4162-840c-7d9a4c50fd66" />   


## Actividad 5  
<a name="actividad 5"></a>
- Link del repositorio:  
  https://github.com/mafora12/actividad_5.git


## Autoevaluación   

| Actividades | Nota | justificación |
| --- | --- | --- |
| 1 | 5.0 | Se evidencia la [actividad 1](#actividad 1) completa, con las 4  (p1, p2, p3, p4) preguntas completadas y su respectiva verificación (v1 y v2) |
| 2 | 5.0 | Se evidencia la [actividad  2](#actividad 2)completa, con las 8 preguntas  (p5, p6, p7, p8)  completadas y su respectiva verificación (v3) |
| 3 | 5.0 | Se evidencia  la [actividad 3](#actividad 3)completa, con las 4 preguntas  (p9, p10, p11, p12)  completadas |
| 4 | 5.0 | Se puede evidenciar la [actividad 4](#actividad 4)completa, con los mapas (v4, v5, v6)  completados. |
| 5 | 5.0 | Se muestra la [actividad 5](#actividad 5) completa, con 4l boceto de la idea (v7) y el desarrollo del código (p13) |
| nota final del promedio | 5.0 |  |
  




