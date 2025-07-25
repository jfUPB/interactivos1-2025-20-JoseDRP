# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01

#### - ¿Qué es un sistema físico interactivo?

Un sistema físico interactivo es un conjunto de componentes tecnológicos que permiten a los usuarios interactuar de manera física y en tiempo real con una experiencia digital, como en videojuegos, simuladores o instalaciones artísticas interactivas. Estos sistemas detectan acciones del usuario y responden generando una reacción visual, sonora o física que enriquece una experiencia predeterminada.

#### - ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

Como estudiante de esta carrera, puedo aplicar lo aprendido sobre sistemas físicos interactivos en el diseño y desarrollo de experiencias inmersivas, como videojuegos que respondan al movimiento del jugador mediante sensores, o instalaciones interactivas para eventos, museos o parques temáticos. También puedo usar este conocimiento para integrar hardware como controladores personalizados, realidad aumentada o realidad virtual que mejoren la conexión física entre el usuario y el entorno digital. Esto me permite crear experiencias más atractivas, intuitivas y memorables, combinando software, diseño y hardware de forma innovadora.

### Actividad 02

#### - ¿Qué es el diseño/arte generativo?

El arte o diseño generativo es un proceso creativo en el que el artista o diseñador crea un sistema que luego genera las obras por sí mismo con IA. En lugar de hacer cada parte manualmente, se programa un algoritmo que produce imágenes, sonidos o experiencias que pueden ser únicas cada vez que se ejecutan.

#### - ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

El arte y diseño generativo, basado en IA, algoritmos y sistemas autónomos, puede potenciar mi perfil profesional al aplicarse en áreas como videojuegos (generación procedural de niveles, música adaptativa) o inteligencia artificial creativa (NPCs, arte conceptual, narrativa dinámica). Al combinar tu formación técnica con procesos creativos automatizados, puedes crear experiencias únicas, escalables e innovadoras, fortaleciendo tu portafolio y perfil profesional en industrias como videojuegos, medios inmersivos y tecnologías interactivas.

### Actividad 03

Luego de hacer el proceso de exploración con la tarjeta micro:bit, estas son mis respuestas:

- En el punto 15 vemos que el Input son los botones con los que enviamos información desde la tarjeta hasta el sistema para que realice una salida por medio de la pantalla que sería el Output. Las instrucciones de este ejercicio son, en su mayoría, realizadas por la siguiente línea de código:


```py
function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');
        }
        else if(dataRx == 'B'){
            fill('yellow');
        }
        else{
            fill('green');
        }
        background(220);
        ellipse(width / 2, height / 2, 100, 100);
        fill('black');
        text(dataRx, width / 2, height / 2);
    }
```

- Tal y como vimos en el punto 15, en el punto 16 también tenemos una entrada; sin embargo, este se diferencia de la anterior en que el dispositivo de entrada parece ser un sensor de movimiento. Al agitar la tarjeta micro:bit, recibimos un output pintando un color verde en la pantalla.

-  En el punto 17, vemos un proceso diferente de los 2 puntos anteriores, pues ahora el dispositivo de entrada ahora es el mouse con el que presionamos el botón virtual en el editor de código. Al presionar el botón, recibimos una salida en los leds de la tarjeta micro:bit, pintando un corazón, seguido de una cara feliz.
