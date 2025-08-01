# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01:

**1.** Este código crea una clase `Pixel` que representa un LED en la tarjeta microbit, el cual puede parpadear en diferentes intervalos de tiempo. Cada objeto `Pixel` usa una máquina de estados. En el estado `Init` enciende el LED y guarda el tiempo, y en `WaitTimeout` alterna el brillo entre 0 (apagado) y 9 (encendido) cada cierto intervalo (en milisegundos). En el bucle principal, se actualizan dos píxeles en posiciones distintas que parpadean con diferentes velocidades.

**2.** El programa tiene 2 estados principales. El primero es `Init`, que es el estado de inicialización del píxel. El segundo es `WaitTimeout` que funciona en un estado de espera, donde el programa monitorea si ha pasado el intervalo de tiempo necesario.

**3.** El programa tiene 2 inputs/eventos principales. El primero que se da al iniciar la ejecución, este simplemente inicializa los LEDs y cambia el estado a WaitTimeout. El segundo es el principal, `Interval` donde se alterna el estado del LED entre encendido/apagado; este es constantemente monitoreado en el estado `WaitTimeout`

**4.** Estas son las acciones que se evidencian en el programa al ejecutarlo:

- **Inicializar el píxel:** Guarda el tiempo actual `startTime` con `utime.ticks_ms()`. También, muestra el estado inicial del LED en la posición `pixelX, pixelY` usando `display.set_pixel()`. Finalmente, cambia el estado interno de la máquina de `Init` a `WaitTimeout`.

- **Verificar el paso del tiempo:** Compara el tiempo actual con `startTime` para saber si pasó el tiempo definido. Si pasó el intervalo, actualiza `startTime` con el nuevo tiempo actual.

- **Alternar el estado del LED:** Cambia el brillo del LED entre 9 (encendido) y 0 (apagado). Luego, refleja ese cambio con `display.set_pixel(pixelX, pixelY, pixelState)`.

- **Actualizar el estado interno:** Mantener o cambiar el estado `Init` o `WaitTimeout` según corresponda.


### Actividad 02:

**Paso 1:**

- Lo primero que hice fue definir claramente los estados en los que el semáforo puede estar. Para un semáforo simple, los estados son Rojo, Amarillo y Verde.

**Paso 2:**

- Luego, establecí cuántos segundos debe durar cada luz del semáforo antes de cambiar. Estos tiempos son típicos para simular un semáforo básico y proporcionan un ciclo adecuado de cambio entre las luces. Por tanto, usé tiempos peredeterminados para cada estado:

Rojo = 3seg
Amarillo = 1seg
Verde = 5seg

**Paso 3:**

- Luego de esto, se debe crear una máquina de estados que maneje la lógica de cambiar entre los estados de semáforo. Por lo que, implementé una clase llamada Semaforo para manejar el cambio de estados. Inicialicé el semáforo en el estado Rojo Y Usé un temporizador con ayuda del código de la actividad anterior `start_time = utime.ticks_ms()` para controlar cuánto tiempo ha pasado desde que el semáforo empezó en su estado actual.

**Paso 4:**

- Después, hay que programar el cambio de un estado a otro después de que haya pasado un cierto tiempo. En cada ciclo de actualización (actualizar()), el programa revisa el estado actual del semáforo Y Si el tiempo transcurrido desde start_time supera el tiempo configurado para el estado, el semáforo cambia al siguiente estado:

***De Rojo a Verde. De Verde a Amarillo. De Amarillo a Rojo.***

- Reinicié el temporizador `start_time = utime.ticks_ms()` cada vez que el semáforo cambia de estado.

**Paso 5:**

- En cada estado, el semáforo se debe mostrar una representación visual adecuada en la pantalla ya que los LEDs de la micro:bit solo tienen color rojo.

- **Rojo:** Mostré un ***corazón*** en la pantalla para simular la luz roja. 
- **Amarillo:** Mostré un ***cuadrado*** en la pantalla para simular la luz amarilla. 
- **Verde:** Mostré una ***cara feliz*** en la pantalla para simular la luz verde.

**Paso 6:**

- Luego, implementé un bucle while True que ejecuta continuamente el método actualizar() de la clase Semaforo. Esto asegura que el semáforo cambie de estado en función del tiempo y repita el ciclo.

**Paso 7:** ***Código Final***

- Este es el código final:

```py
from microbit import *
import utime

ROJO = 0
AMARILLO = 1
VERDE = 2

TIEMPO_ROJO = 3000
TIEMPO_AMARILLO = 1000
TIEMPO_VERDE = 5000

class Semaforo:
    def __init__(self):
        self.estado = ROJO
        self.start_time = utime.ticks_ms()

    def actualizar(self):
        if self.estado == ROJO:
            display.show(Image.HEART)
            if utime.ticks_diff(utime.ticks_ms(), self.start_time) > TIEMPO_ROJO:
                self.estado = VERDE
                self.start_time = utime.ticks_ms()

        elif self.estado == AMARILLO:
            display.show(Image.SQUARE)
            if utime.ticks_diff(utime.ticks_ms(), self.start_time) > TIEMPO_AMARILLO:
                self.estado = ROJO
                self.start_time = utime.ticks_ms()

        elif self.estado == VERDE:
            display.show(Image.HAPPY)
            if utime.ticks_diff(utime.ticks_ms(), self.start_time) > TIEMPO_VERDE:
                self.estado = AMARILLO
                self.start_time = utime.ticks_ms()

semaforo = Semaforo()

while True:
    semaforo.actualizar()
```
