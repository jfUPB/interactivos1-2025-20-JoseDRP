# Unidad 1

## 🛠 Fase: Apply

### Actividad 05

- Lo primero que hacemos en el código es importar la biblioteca de micro:bit para poder usar todas sus funciones:

 ```py
from microbit import *
 ```

- Luego de esto, inicializamos la comunicación serial para enviar información desde nuestro PC a la tarjeta. Según lo explicado, 115200 es la velocidad de transferencia de datos y esta es una velocidad comúnmente utilizada para este tipo de comunicación serial. También, usamos el `button_a.is_pressed()` para detectar si el botón está siendo presionado; en este caso es mejor que `button_a.was_pressed()` debido a que necesitamos saber si el botón está siendo presionado en ese momento en específico; así, si dejamos presionado el botón, se puede enviar múltiples mensajes constantemente:

 ```py
    from microbit import *

  while True:
      if button_a.was_pressed():
 ```

- Luego, en p5.js, creamos un par de variables globales para manipular la conexión con el puerto serial y para conectar y desconectar la tarjeta micro:bit de la aplicación. También creamos el canvas necesario dentro de la función `setup()` donde se mostrará más adelante el cuadrado.

 ```js
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
}
 ```

- Finalmente, creamos el cuadrado en la función `draw()` y añadimos la funcionalidad; al recibir una A como dato, el cuadrado se pintará de color rojo. Si no se recibe ningún dato, el cuadrado se quedará de color verde. También le damos la funcionalidad al botón para conectar y desconectar la tarjeta micro:bit en cualquier momento:

 ```js
  function draw() {
        background(220);

        if (port.availableBytes() > 0) {
            let dataRx = port.read(1);
            if (dataRx == "A") {
            fill("red");
            }
        } else {
            fill("green");
        }

        rectMode(CENTER);
        rect(width / 2, height / 2, 50, 50);

        if (!port.opened()) {
            connectBtn.html("Connect to micro:bit");
        } else {
            connectBtn.html("Disconnect");
        }
    }
 ```

- Así quedarían ambos códigos para el funcionamiento correcto del programa, donde el input es el botón cuando se presiona; y el output es el color del cuadrado dependiendo si lo estamos presionando o no:

**Micro:bit**: 

```py
from microbit import *

uart.init(baudrate=115200)

while True:

    if button_a.is_pressed():
        uart.write('A')
    else:
        uart.write('N')

    sleep(100)
 ```

**p5.js**:

```js
 let port;
  let connectBtn;
  let connectionInitialized = false;

  function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton("Connect to micro:bit");
    connectBtn.position(80, 300);
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
      if (dataRx == "A") {
        fill("red");
      } else if (dataRx == "N") {
        fill("green");
      }
    }

    rectMode(CENTER);
    rect(width / 2, height / 2, 50, 50);

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

### Actividad 06

- Estos fueron los códigos finales con los que logré finalizar correctamente el problema propuesto en clase:

[**Link al programa**](https://editor.p5js.org/JoseDRP/sketches/JhD5pxWLL)

**p5.js**:

```js
let port;
  let connectBtn;

  let x = 200;
  let y = 200;


  function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton("Connect to micro:bit");
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
  }

  

  function draw() {
    background(220);
    
    
    if (port.availableBytes() > 0) {
      let dataRx = port.read(1);
      if (dataRx == "A") {
        x = x - 1;
      }
      if (dataRx == "B") {
        x = x + 1;
      }
    }

    circle(x, y, 50);

    if (!port.opened()) {
      connectBtn.html("Connect to micro:bit");
    } else {
      connectBtn.html("Disconnect");
    }
  }

  function connectBtnClick() {
    if (!port.opened()) {
      port.open("MicroPython", 115200);
    } else {
      port.close();
    }
  }
```
**Micro:bit**: 

```py
from microbit import *

uart.init(baudrate=115200)

while True:

    if button_a.was_pressed():
        uart.write('A')
    if button_b.was_pressed():
        uart.write('B')

 ```
