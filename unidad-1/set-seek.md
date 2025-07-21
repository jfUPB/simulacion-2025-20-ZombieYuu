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

```javascript
// The Nature of Code
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

```javascript
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = random( 10);
    if (choice <=7) {
      this.x++;
    } else if (choice <= 8) {
      this.x--;
    } else if (choice <= 9) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```

### Actividad 5:

```javascript
function setup() {
  createCanvas(600, 400);
  background(0);
  noStroke();
  fill(255, 100);

  for (let i = 0; i < 10000; i++) {
    let x = randomGaussian(width / 2, 60);
    let y = randomGaussian(height / 2, 60);
    ellipse(x, y, 2, 2);
  }
}
```

*Link p5js:* https://editor.p5js.org/ZombieYuu/sketches/W2P8apaF7

<img width="745" height="497" alt="image" src="https://github.com/user-attachments/assets/4a7012b1-2a74-4553-af49-18a92460e390" />

### Actividad 6:

```javascript
function setup() {
  createCanvas(600, 400);
  background(0);
  noStroke();

  // --- Distribución Normal (azul)
  fill(100, 200, 255, 80);
  for (let i = 0; i < 5000; i++) {
    let x = randomGaussian(width / 3, 40);
    let y = randomGaussian(height / 2, 40);
    ellipse(x, y, 2, 2);
  }

  // --- Lévy Flight (rosado)
  fill(255, 150, 255, 80);
  let x = width * 2 / 3;
  let y = height / 2;

  for (let i = 0; i < 5000; i++) {
    let angle = random(TWO_PI);
    let step = pow(random(1), -1.5) * 5; // pasos largos ocasionales
    x += cos(angle) * step;
    y += sin(angle) * step;

    x = constrain(x, 0, width);
    y = constrain(y, 0, height);

    ellipse(x, y, 2, 2);
  }
}
```
*Link p5js:* https://editor.p5js.org/ZombieYuu/sketches/kBfxx2tfo

<img width="751" height="497" alt="image" src="https://github.com/user-attachments/assets/7cd6fe9c-d0e4-4254-bf7a-cf5bc978f367" />

Esta técnica fue utilizada para comparar doos nubes de punos, la simétrica y la concentrada, espero que la nube normal esté mas concentrada en el centro mientras que la nube Levy flght como salta lejos del centro y rompe las simetría se verá de una forma mas uniforme.

### Actividad 7:

```javascript
let yoff = 0;

function setup() {
  createCanvas(600, 400);
  background(0);
}

function draw() {
  background(20);
  stroke(100, 200, 255);
  noFill();

  beginShape();
  let xoff = 0;
  for (let x = 0; x < width; x += 10) {
    let y = map(noise(xoff, yoff), 0, 1, 100, 300);
    vertex(x, y);
    xoff += 0.05;
  }
  endShape();

  yoff += 0.01;
}
```

*Link p5js:* https://editor.p5js.org/ZombieYuu/sketches/yW1VCYsQC

<img width="734" height="493" alt="image" src="https://github.com/user-attachments/assets/645da407-62ec-4f59-87ed-72ebd9c197b6" />

Esperaba ver una onda que su ocilación fiera suave y cambiara lentamente en el tiempo.
