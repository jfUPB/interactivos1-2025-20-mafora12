# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1


¿Qué es un sistema físico interactivo?  

   Es un objeto que al sentir una acción o interaccion del usuario, lo procesa y lo convierte en una proyección.
   Tiene una entrada física que se denomina input, un procesador y una salida denominada output. ósea, Es un sistema que responde a una acción que uno hace. Por ejemplo, cuando uno toca algo,
   mueve un sensor o presiona un botón, el sistema lo detecta (eso sería el input), lo procesa con un código o una lógica, y luego hace algo como mostrar una luz, cambiar de color o hacer un sonido (eso sería el output).
   
¿Cómo podrías aplicar lo que has visto en tu perfil profesional?  
   
En diseño gráfico o como ingeniera de entretenimiento digital, estos sistemas pueden hacer que las experiencias sean más inmersivas.
Por ejemplo, puedo crear una instalación donde las ilustraciones se crren a partir de la interacción de alguien, o una animación que reaccione cuando el público interactúe. 
Eso haría que mi trabajo conecte más con las personas y tenga un toque único, generando mayor capacidad de recordaciòn en las personas o publicos.

### Actividad 2  

¿Qué es el diseño/arte generativo? 

Consiste en trabajar con elementos variables bajo ciertas reglas pero con aleatoriedad. Es cualquier práctica de diseño en la que el artista o diseñador 
utiliza un sistema con un conjunto de reglas (ya sea en lenguaje natural, software o máquinas) para ejecutar una pieza de diseño o arte.
Este proceso, relativamente autónomo, le da un toque único y distintivo a la obra, debido a que aunque no es algo hecho directamente por un ser humano, el codigo que 
se crea si lo es, teniendo una nueva forma de diseñar con la ventaja de tener miles de posibilidades ´para las obras de diseño o arte.

¿Cómo podrías aplicar lo que has visto en tu perfil profesional?  
Como diseñadora, puedo usar esto para crear patrones, fondos, ilustraciones o animaciones que no se repiten y se vean diferentes cada vez. 
Esto me ayudaría a ofrecer cosas más creativas y distintas en mis proyectos, usando herramientas nuevas como el código para crear diseño.
facilitando la creación de piezas graficas o interacciones mas dinamicas y emocionantes.

### Actividad 3  

Cuales son los inputs, outputs y el proceso:  
Browser:  
  - Inputs: Los botones y el acelerometro, writes, el cable (USB) , Display
  - output: el cable (USB) 
  - Proceso: el código que lee esas acciones y decide qué hacer con ellas.
    
Computador:  
  - Input: el cable (USB) , el Boton "send love"
  - Output: Datos y pantalla del computador
  - Proceso: el código que está en el computador recibe eso, lo entiende y reacciona.

### Actividad 4  

Enlace  

[mi codigo](https://editor.p5js.org/mafora12/sketches/eHb6skJjE)   

Codigo:  

```javascript
function setup() {
  createCanvas(500, 500);
  background(230, 200, 255); 
}

function draw() {
  
  fill(1080, 200, 255, 20);
  noStroke();
  rect(0, 0, width, height);

  let x = 100 * cos(frameCount * 0.05) + 200;
  let y = 50 * sin(frameCount * 0.1) + 200;

  let w = 20 + 5 * sin(frameCount * 0.2);
  let h = 10 + 2.5 * sin(frameCount * 0.2);

  
  fill(255, 90, 200, 200); 
  noStroke();
  ellipse(x, y, w, h);
}
```

Imagen:  

<img width="627" height="650" alt="image" src="https://github.com/user-attachments/assets/7c0455d1-67e7-4921-9565-fdda558ad639" />

