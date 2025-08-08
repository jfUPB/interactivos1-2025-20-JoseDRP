# Unidad 2

## 🛠 Fase: Apply

### Actividad 04

Este es el diagrama de máquinas de estado que hice para este problema:

<img width="742" height="650" alt="mapa drawio" src="https://github.com/user-attachments/assets/fed03953-288e-4d1e-8679-8fb7dcacb612" />

### Actividad 05

Este es el código:

```py
from microbit import *
import music
import utime

# Estado inicial
config_mode = True
time_left = 20 
min_time = 10
max_time = 60
armed = False
last_update = utime.ticks_ms()

def display_time(t):
    display.show(str(t % 10)) # Solo muestra un dígito

def explosion():
    display.show(Image.SKULL)
    music.play(music.WAWAWAWAA)
    sleep(2000)
    display.clear()

while True:
    if config_mode:
        display_time(time_left)

        if button_a.was_pressed():
            if time_left < max_time:
                time_left += 1

        if button_b.was_pressed():
            if time_left > min_time:
                time_left -= 1

        if accelerometer.was_gesture('shake'):
            config_mode = False
            armed = True
            last_update = utime.ticks_ms()
            display.clear()

    elif armed:
        # Actualiza cada segundo
        current = utime.ticks_ms()
        if utime.ticks_diff(current, last_update) >= 1000:
            time_left -= 1
            last_update = current
            if time_left > 0:
                display_time(time_left)
            else:
                explosion()
                armed = False
                config_mode = True
                time_left = 20 

        # Volver a modo configuración si se toca el botón touch (pin0)
        if pin0.is_touched():
            armed = False
            config_mode = True
            time_left = 20
            display.clear()

```

