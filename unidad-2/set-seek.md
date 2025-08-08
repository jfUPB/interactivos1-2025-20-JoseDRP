# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01

**1.** Este código crea una clase `Pixel` que representa un LED en la tarjeta microbit, el cual puede parpadear en diferentes intervalos de tiempo. Cada objeto `Pixel` usa una máquina de estados. En el estado `Init` enciende el LED y guarda el tiempo, y en `WaitTimeout` alterna el brillo entre 0 (apagado) y 9 (encendido) cada cierto intervalo (en milisegundos). En el bucle principal, se actualizan dos píxeles en posiciones distintas que parpadean con diferentes velocidades.

**2.** El programa tiene 2 estados principales. El primero es `Init`, que es el estado de inicialización del píxel. El segundo es `WaitTimeout` que funciona en un estado de espera, donde el programa monitorea si ha pasado el intervalo de tiempo necesario.

**3.** El programa tiene 2 inputs/eventos principales. El primero que se da al iniciar la ejecución, este simplemente inicializa los LEDs y cambia el estado a WaitTimeout. El segundo es el principal, `Interval` donde se alterna el estado del LED entre encendido/apagado; este es constantemente monitoreado en el estado `WaitTimeout`

**4.** Estas son las acciones que se evidencian en el programa al ejecutarlo:

- **Inicializar el píxel:** Guarda el tiempo actual `startTime` con `utime.ticks_ms()`. También, muestra el estado inicial del LED en la posición `pixelX, pixelY` usando `display.set_pixel()`. Finalmente, cambia el estado interno de la máquina de `Init` a `WaitTimeout`.

- **Verificar el paso del tiempo:** Compara el tiempo actual con `startTime` para saber si pasó el tiempo definido. Si pasó el intervalo, actualiza `startTime` con el nuevo tiempo actual.

- **Alternar el estado del LED:** Cambia el brillo del LED entre 9 (encendido) y 0 (apagado). Luego, refleja ese cambio con `display.set_pixel(pixelX, pixelY, pixelState)`.

- **Actualizar el estado interno:** Mantener o cambiar el estado `Init` o `WaitTimeout` según corresponda.


### Actividad 02

**Paso 1:**

- Lo primero que hice fue definir claramente los estados en los que el semáforo puede estar. Para un semáforo simple, los estados son Rojo, Amarillo y Verde.

**Paso 2:**

- Luego, establecí cuántos segundos debe durar cada luz del semáforo antes de cambiar. Estos tiempos son típicos para simular un semáforo básico y proporcionan un ciclo adecuado de cambio entre las luces. Por tanto, usé tiempos peredeterminados para cada estado:

 - Rojo = 3seg
 - Amarillo = 1seg
 - Verde = 5seg

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

### Actividad 03

**Explicación de concurrencia en el programa**

Este programa para la micro:bit simula un sistema de máquina de estados finitos en el que la pantalla cambia entre diferentes expresiones faciales en función del tiempo y la interacción con el botón A.

- **El uso de una máquina de estados:** El código mantiene un seguimiento del estado actual y cambia de estado en función del tiempo o la interacción del usuario.

- **Temporización con utime.ticks_ms():** En lugar de detener la ejecución con sleep(), se usa la diferencia de tiempo transcurrido para decidir cuándo cambiar de estado.

- **Manejo de eventos con button_a.was_pressed():** Se revisa constantemente si el botón A ha sido presionado, permitiendo una reacción inmediata sin bloquear el flujo del programa.

De esta manera, el programa parece realizar múltiples tareas a la vez: controla el tiempo para los cambios automáticos de estado y también responde a las interacciones del usuario sin que una tarea interrumpa la otra.

En resumen, el código hace que la pantalla de la micro:bit cambie de cara automáticamente con el tiempo y también reaccione cuando presionas el botón A. El programa revisa rápidamente dos cosas en un bucle:

 - Si ha pasado el tiempo para cambiar de cara.
 - Si presionaste el botón.

Como revisa estas dos cosas muchas veces por segundo, parece que está haciendo ambas al mismo tiempo. Eso es lo que llamamos concurrencia en este caso.

**Vectores de prueba**

Cada vector de prueba consta de:

- **Condiciones iniciales:** Estado actual del sistema antes de iniciar la prueba.
- **Eventos generados:** Botón presionado o tiempo transcurrido.
- **Resultados esperados:** Estado final y la imagen mostrada.
- **Resultados obtenidos:** Estado final y la imagen mostrada después de ejecutar la prueba.

**Vector de prueba 1: Transición automática de HAPPY a SMILE**

- **Condiciones iniciales:** El sistema está en estado STATE_HAPPY con la imagen 😊.
- **Evento generado:** Se espera que pase 1500 ms sin presionar ningún botón.
- **Resultado esperado:** Cambio al estado STATE_SMILE, mostrando la imagen 😀.
- **Resultado obtenido:** Correcto (El sistema cambió de HAPPY a SMILE automáticamente).

**Vector de prueba 2: Interrupción manual de SMILE a HAPPY**

- **Condiciones iniciales:** El sistema está en estado STATE_SMILE con la imagen 😀.
- **Evento generado:** Se presiona el botón A antes de que pasen los 1000 ms del intervalo de SMILE.
- **Resultado esperado:** Cambio inmediato al estado STATE_HAPPY, mostrando la imagen 😊.
- **Resultado obtenido:** Correcto (El sistema respondió al botón sin esperar el tiempo programado).

**Vector de prueba 3: Transición forzada de HAPPY a SAD**

- **Condiciones iniciales:** El sistema está en estado STATE_HAPPY con la imagen 😊.
- **Evento generado:** Se presiona el botón A.
- **Resultado esperado:** Cambio inmediato al estado STATE_SAD, mostrando la imagen ☹️ y estableciendo un temporizador de 2000 ms.
- **Resultado obtenido:** Correcto (El botón cambió el estado sin esperar el tiempo programado).

