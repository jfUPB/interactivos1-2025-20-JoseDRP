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

