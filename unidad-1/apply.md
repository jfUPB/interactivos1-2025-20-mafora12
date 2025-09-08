# Unidad 1

## 🛠 Fase: Apply


### Actividad 5  
Si oprimo el cuadro en el canva esta blanco y al conctarse al micro:bit se vuelve verde. Si oprimo el boton A del micro:bit se vueleve el cuadro rojo y si lo dejo de oprimir se vuelve verde.  

- Imput: botón A, información del serial
- Output: Programa, lo visual
- Procesador:  si recibo un A lo pinto rojo si dejo de hundir esta verde (programa)

### Actividad 6
[codigo p5](https://editor.p5js.org/mafora12/sketches/lXU1OAV6-)  

Codigo p5:  

```javascript
let port;
let connectBtn;
let connectionInitialized = false;

let circleX = 200; 
let moveStep = 10; 

function setup() {
  createCanvas(400, 400);
  background(220);

  port = createSerial();

  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(80, 350);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);


  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  
  if (port.availableBytes() > 0) {
    let dataRx = port.read(1); 

    if (dataRx === "A") {
      circleX -= moveStep;
    } else if (dataRx === "B") {
      circleX += moveStep;
    }
  }


  circleX = constrain(circleX, 25, width - 25);

  fill("pink");
  ellipse(circleX, height / 2, 50, 50);


  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

```

Codigo micro:bit :  
```py
from microbit import *

uart.init(baudrate=115200)
display.show(Image.GIRAFFE)

while True:
    if button_a.is_pressed():
        uart.write('A')
    elif button_b.is_pressed():
        uart.write('B')
    else:
        uart.write('N')
    
    sleep(100)
```
