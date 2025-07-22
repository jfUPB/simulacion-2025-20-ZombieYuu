# Unidad 1

## 🛠 Fase: Apply

### Actividad 8: 
*Mi obra generativa:* Quiero realizar una experiencia donde aparezcan dos serpientes en la pantalla, y que cada una se mueva diferente conforme a una distribución normal y una no uniforme respectivamente, las serpientes se moverán siguiendo el mouse y al presionar la tecla espaciadora cambiarán de color.

*Código:*

```javascript
let snake1 = [];
let snake2 = [];
let len = 50;
let noiseScale1 = 0.02;
let noiseScale2 = 0.05;

let t1 = 0;
let t2 = 1000;

let colors1 = [];
let colors2 = [];

function setup() {
  createCanvas(800, 600);
  colorMode(HSB, 360, 100, 100);

  // Inicializamos las serpientes separadas horizontalmente
  for (let i = 0; i < len; i++) {
    snake1.push(createVector(width / 3, height / 2));  // A la izquierda
    snake2.push(createVector((2 * width) / 3, height / 2));  // A la derecha
  }

  colors1 = [random(360), random(50, 100), random(50, 100)];
  colors2 = [random(360), random(50, 100), random(50, 100)];
}

function draw() {
  background(20);

  updateSnake1();
  drawSnake(snake1, colors1);

  updateSnake2();
  drawSnake(snake2, colors2);
}

function updateSnake1() {
  let head = snake1[0].copy();
  let target = createVector(mouseX, mouseY);

  let dir = p5.Vector.sub(target, head);
  let maxStep = 6; // Más rápida para esta serpiente
  if (dir.mag() > maxStep) {
    dir.setMag(maxStep);
  }
  snake1[0] = p5.Vector.add(head, dir);

  for (let i = 1; i < len; i++) {
    let prev = snake1[i - 1].copy();

    let angleNoise = noise(prev.x * noiseScale1, prev.y * noiseScale1, t1 + i * 0.1);
    let biasedNoise = pow(angleNoise, 3);
    let angle = map(biasedNoise, 0, 1, -PI, PI);
    let step = p5.Vector.fromAngle(angle).mult(5);
    snake1[i] = p5.Vector.lerp(snake1[i], p5.Vector.add(prev, step), 0.5);
  }

  t1 += 0.015; // También puede aumentar la velocidad del tiempo para ruido
}

function updateSnake2() {
  let head = snake2[0].copy();
  let target = createVector(mouseX, mouseY);

  let dir = p5.Vector.sub(target, head);
  let maxStep = 3; // Más lenta para esta serpiente
  if (dir.mag() > maxStep) {
    dir.setMag(maxStep);
  }
  snake2[0] = p5.Vector.add(head, dir);

  for (let i = 1; i < len; i++) {
    let prev = snake2[i - 1].copy();

    let baseNoise = noise(prev.x * noiseScale2, prev.y * noiseScale2, t2 + i * 0.1);
    let meanAngle = map(baseNoise, 0, 1, -PI / 4, PI / 4);
    let stddev = 0.5;
    let angle = meanAngle + gaussianRandom(0, stddev);
    let step = p5.Vector.fromAngle(angle).mult(5);
    snake2[i] = p5.Vector.lerp(snake2[i], p5.Vector.add(prev, step), 0.5);
  }

  t2 += 0.01;
}

function drawSnake(snake, col) {
  stroke(col[0], col[1], col[2]);
  strokeWeight(8);
  noFill();
  beginShape();
  for (let v of snake) {
    vertex(v.x, v.y);
  }
  endShape();
}

function keyPressed() {
  if (key === ' ') {
    colors1 = [random(360), random(50, 100), random(50, 100)];
    colors2 = [random(360), random(50, 100), random(50, 100)];
  }
}

function gaussianRandom(mean = 0, stdDev = 1) {
  let u = 1 - random();
  let v = random();
  let z = sqrt(-2.0 * log(u)) * cos(TWO_PI * v);
  return z * stdDev + mean;
}
```

*Enlace p5js:* https://editor.p5js.org/ZombieYuu/full/M135DyS3w

<img width="798" height="596" alt="image" src="https://github.com/user-attachments/assets/2eefced5-0f35-4058-9eef-397e41a48cdf" />
