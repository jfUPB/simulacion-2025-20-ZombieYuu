# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Utilicé el concepto de honda, ocilación y sinusoides con amplitud de onda para simular el movimiento de un dragón Chino volando en el aire.
>

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> De esta unidad utilizo la recistencia del aire, lo cual hace que el dragón se mueva mas lento.
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Utilicé el concepto de aceletación, al presionar la tecla ¨a¨ el dragón acelera. 
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Con una distribución normal hice que el movimiento se volviera  mas orgánico.
>

## ¿Cómo resolviste la interacción?
> Tuve varios desafíos para poder conseguir el resultado esperado, primero el dragon tenia una vibración mas alta de lo que esperaba por la distribución normal, entondes lo suavicé un poco reduciendo el ``randomGaussian`` y también tuve que suavizar el movimiento de la cabeza.
>

## Enlace a la obra en el editor de p5.js

https://editor.p5js.org/ZombieYuu/sketches/Ky2OtC4V5

## Código de la obra 

``` js
let numSegments = 20;
let segmentLength = 20;
let segments = [];

let acceleration = 0.02; // Menor aceleración para la cabeza
let maxSpeed = 2;        // Limita cuánto puede moverse la cabeza
let dragCoefficient = 0.1;

let accelerationKeyPressed = false;

let waveOffset = 0;

function setup() {
  createCanvas(800, 600);
  for (let i = 0; i < numSegments; i++) {
    segments.push(createVector(width / 2, height / 2));
  }
}

function draw() {
  background(240);

  waveOffset += 0.1; // Avanza el ángulo para la onda

  // HEAD SEGMENT: Movimiento más suave hacia el mouse
  let target = createVector(mouseX, mouseY);
  let head = segments[0];

  // Suavizado: usar interpolación en lugar de fuerza directa
  head.set(p5.Vector.lerp(head, target, acceleration));

  // Opcional: puedes usar física si querés mantenerlo con velocidad y resistencia
  // Pero la interpolación suaviza naturalmente

  // BODY SEGMENTS FOLLOW WITH OSCILLATION
  for (let i = 1; i < segments.length; i++) {
    let prev = segments[i - 1];
    let curr = segments[i];

    // Dirección hacia el anterior
    let direction = p5.Vector.sub(prev, curr);
    direction.normalize();
    direction.mult(segmentLength);

    // Oscilación como onda que recorre el cuerpo
    let angle = waveOffset - i * 0.5; // Desfase por segmento
    let amplitude = 4;
    let oscillation = createVector(-direction.y, direction.x); // Perpendicular
    oscillation.normalize().mult(sin(angle) * amplitude);

    // Nueva posición con oscilación
    curr.set(p5.Vector.sub(prev, direction).add(oscillation));
  }

  // DRAW THE DRAGON AS INTERCALATED TRIANGLES
  noStroke();
  for (let i = 0; i < segments.length - 1; i++) {
    let curr = segments[i];
    let next = segments[i + 1];

    let dir = p5.Vector.sub(next, curr);
    let angle = atan2(dir.y, dir.x);

    let base = map(i, 0, segments.length, 30, 12);

    push();
    translate(curr.x, curr.y);
    rotate(angle);

    // Alternar entre rojo y amarillo
    if (i % 2 === 0) {
      fill(255, 0, 0); // Rojo
    } else {
      fill(255, 204, 0); // Amarillo
    }

    // Dibujar triángulo
    beginShape();
    vertex(0, -base / 2);
    vertex(segmentLength, 0);
    vertex(0, base / 2);
    endShape(CLOSE);

    pop();
  }
}

function keyPressed() {
  if (key === 'a' || key === 'A') {
    accelerationKeyPressed = true;
  }
}

function keyReleased() {
  if (key === 'a' || key === 'A') {
    accelerationKeyPressed = false;
  }
}

```

## Captura de pantalla representativa

<img width="787" height="575" alt="image" src="https://github.com/user-attachments/assets/e613b36d-7dc2-459a-abdd-08401b3a2309" />






