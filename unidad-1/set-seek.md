# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1:

El sistema aleatorio influye en el arte generativo al generar un toque de novedad y dinamismo que mantiene al expectador atento al espectáculo del arte generativo.

### Actividad 2:

En el trabajo de Sofia el factor aleatorio se infunde a partir del movimiento y expansión de las ramas co nforme la música cambia de ritmo, manteniendo al espectador atento y entretenido durante la experiencia.
En mi ámbito profecional la aleatoriedad juega un papael al usar diversas formas para conformar el boceto de un dibujo}

### Actividad 3:
Al experimentar con el código de Traditional Random Walk para que el dibujo empezara en un lugar aleatorio del lienzo vi que mientras mayor sea el numero entre width y heit es menor el rango de movimiento mientas que mientras mas pequeño sea el número mayor rango de movimiento entre ejecución.

### Código de experimentacion:

```// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(600, 600);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / random(6);
    this.y = height / random(5);
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```

### Actividad 4:

En una distribución uniforme los numeros aleatorios tienen la misma probabilidad de salir en un mismo plano, mientras que en una no uniforme depende de la desviación estandar para su probabilidad.
