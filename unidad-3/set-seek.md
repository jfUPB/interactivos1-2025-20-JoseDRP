# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 05

1. Estos son los códigos de la bomba 3.0 funcional:

**p5.js:**

```js
let port;
let connectBtn;
let connectionInitialized = false;

let validChars = "ABST";

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

  textAlign(CENTER);
  text("Press A,B,S,T to simulate micro:bit keys", width / 2, height / 2);




  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
}

function keyPressed() {
  keyValue = key.toUpperCase();
  if(validChars.includes(keyValue)){
    console.log(keyValue);
    port.write(keyValue);
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

**micro:bit**:

```py
from microbit import *
import utime
import radio

display.clear()

class Event:
    def __init__(self):
        self.value = 0

    def write(self,value):
        self.value = value

    def read(self):
        return self.value

    def clear(self):
        self.value = 0

class MicroBitSensors():
    def __init__(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.write("A")
        if button_b.was_pressed():
            event.write("B")
        if accelerometer.was_gesture("shake"):
            event.write("S")
        if pin_logo.is_touched():
            event.write("T")

class RemoteTask:
    def __init__(self):
        uart.init(baudrate=115200)

    def update(self):
        if uart.any():
            data = uart.read(1)
            if data:
                if data[0] == ord('A'):
                    event.write("A")
                if data[0] == ord('B'):
                    event.write("B")
                if data[0]== ord('S'):
                    event.write("S")
                if data[0] == ord('T'):
                    event.write("T")


class RadioRemote:
    def __init__(self):
        radio.config(group=69)
        radio.on

    def update(self):
        message = radio.receive()
        if message:
            if message == "A":
                event.write("A")
            elif message == "B":
                event.write("B")
            elif message == "S":
                event.write("S")
            elif message == "T":
                event.write("T")


class BombTask:
    def __init__(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)

    def update(self):
        if self.state == 'CONFIG':
            if event.read()== "A":
                event.clear()
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if event.read()== "B":
                event.clear()
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if event.read()== "S":
                event.clear()
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if event.read()== "A":
                event.clear()
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1

            if event.read()== "B":
                event.clear()
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):

                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break;
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if event.read()== "T":
                event.clear()
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()
event = Event()
sensors = MicroBitSensors()
remoteTask = RemoteTask()
radioRemote = RadioRemote()

while True:
    radioRemote.update()
    remoteTask.update()
    sensors.update()
    bombTask.update()
```

2. Esta es la tabla con los vectores de prueba:

# Vectores de Prueba

| Estado inicial | Evento disparador | Acciones | Estado final |
|----------------|------------------|----------|--------------|
| CONFIG | Botón A (o mensaje/uart 'A') | Incrementar count hasta máx. 60, mostrar en display | CONFIG |
| CONFIG | Botón B (o mensaje/uart 'B') | Decrementar count hasta mín. 10, mostrar en display | CONFIG |
| CONFIG | Sacudir (o mensaje/uart 'S') | Guardar startTime, pasar a modo armado | ARMED |
| ARMED | Cada 1000 ms | Decrementar count, mostrar en display; si count=0 → mostrar calavera | EXPLODED (cuando llega a 0) |
| ARMED | Botón A | Registrar 'A' en clave, avanzar índice | ARMED |
| ARMED | Botón B | Registrar 'B' en clave, avanzar índice | ARMED |
| ARMED | Clave completa y correcta (A-B-A) | Reinicia count=20, vuelve a CONFIG | CONFIG |
| ARMED | Clave completa incorrecta | Reinicia índice de clave, mantener cuenta | ARMED |
| EXPLODED | Tocar logo (o mensaje/uart 'T') | Reinicia count=20, reinicia tiempo, mostrar número en display | CONFIG |



 
