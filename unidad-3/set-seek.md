# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 9:

Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:

- **Fricción:**
  - **Explica cómo modelaste cada fuerza:**
    Usé cada fuerza (como el movimiento del jugador o la fricción) se representa como un vector que tiene dirección y magnitud. El jugador aplica varias fuerzas con las teclas WASD, mientras que las paredes aplican fricción como una fuerza opuesta a la dirección del movimiento, con distinta magnitud según la zona.
    
  - **Conceptualmente cómo se relaciona la fuerza con la obra generativa:**
    La fuerza juega un papel donde las fuerzas que aplica el usuario y la fuerza de fricción pelean para que la bolita llegue a la parte derecha de la pantalla.

  **Enlace:** https://editor.p5js.org/ZombieYuu/sketches/p7oNOSF02

  **Código:**

```javascript
// Laberinto interactivo con fricción variable

let mover;
let walls = [];
let controls = {};

function setup() {
  createCanvas(800, 400);
  mover = new Mover(50, 50, 10);

  // Crear paredes del laberinto
  walls.push(new Wall(200, 0, 20, 300, 0.2));
  walls.push(new Wall(400, 100, 20, 300, 0.05));
  walls.push(new Wall(600, 0, 20, 300, 0.3));
  walls.push(new Wall(100, 300, 600, 20, 0.1));

  createP('Usa W, A, S, D o flechas para moverte. Llega al otro lado del laberinto.');
}

function draw() {
  background(255);

  handleInput();

  // Aplicar fricción si toca una pared
  for (let wall of walls) {
    wall.show();

    if (wall.checkCollision(mover)) {
      let friction = mover.velocity.copy();
      friction.mult(-1);
      friction.setMag(wall.friction);
      mover.applyForce(friction);
    }
  }

  mover.bounceEdges();
  mover.update();
  mover.show();
}

// ----------------------------

function handleInput() {
  let forceMagnitude = 0.5;

  if (controls['ArrowUp'] || controls['w']) {
    mover.applyForce(createVector(0, -forceMagnitude));
  }
  if (controls['ArrowDown'] || controls['s']) {
    mover.applyForce(createVector(0, forceMagnitude));
  }
  if (controls['ArrowLeft'] || controls['a']) {
    mover.applyForce(createVector(-forceMagnitude, 0));
  }
  if (controls['ArrowRight'] || controls['d']) {
    mover.applyForce(createVector(forceMagnitude, 0));
  }
}

// ----------------------------

function keyPressed() {
  controls[key] = true;
}

function keyReleased() {
  controls[key] = false;
}

// ----------------------------

class Mover {
  constructor(x, y, m) {
    this.position = createVector(x, y);
    this.velocity = createVector();
    this.acceleration = createVector();
    this.mass = m;
    this.radius = m * 4;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  bounceEdges() {
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= -1;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= -1;
    }

    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= -1;
    } else if (this.position.y < this.radius) {
      this.position.y = this.radius;
      this.velocity.y *= -1;
    }
  }

  show() {
    fill(50, 100, 200);
    noStroke();
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }
}

// ----------------------------

class Wall {
  constructor(x, y, w, h, friction) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.friction = friction;
  }

  show() {
    fill(150);
    noStroke();
    rect(this.x, this.y, this.w, this.h);
  }

  checkCollision(mover) {
    return (
      mover.position.x + mover.radius > this.x &&
      mover.position.x - mover.radius < this.x + this.w &&
      mover.position.y + mover.radius > this.y &&
      mover.position.y - mover.radius < this.y + this.h
    );
  }
}
```

 **Imagen:**

https://github.com/user-attachments/assets/3bcccf26-91df-451a-bf4f-95f3084d9bb6


- **Resistencia del aire y de fluidos:**
  - **Explica cómo modelaste cada fuerza:**
    Se aplica una fuerza que es opuesta a la velocidad, proporcional a la velocidad cuadrada en el pez, mientras que las estrellas ejercen una fuerza de atracción.
    
  - **Conceptualmente cómo se relaciona la fuerza con la obra generativa:**
    Mientras el pez se acerca a la estrella por la fuerza de atracción, la resistencia lo "jala" como una fricción.

  **Enlace:** [https://editor.p5js.org/ZombieYuu/sketches/p7oNOSF02](https://editor.p5js.org/ZombieYuu/sketches/bMb1NZdGb)

  **Código:**

```javascript
let pez;
let estrellas = [];

function setup() {
  createCanvas(640, 360);
  pez = new Mover(width / 2, height / 2, 3);
  textAlign(CENTER);
  textSize(16);
  createP('El pez sigue la estrella de mar. Haz click para crear una estrella.');
}

function draw() {
  background(200, 230, 255);

  // Mostrar todas las estrellas
  for (let i = estrellas.length - 1; i >= 0; i--) {
    estrellas[i].show();
    // Si el pez llegó a la estrella, la quitamos
    if (p5.Vector.dist(pez.position, estrellas[i].position) < 20) {
      estrellas.splice(i, 1);
    }
  }

  if (estrellas.length > 0) {
    // El pez busca la estrella más cercana
    let target = getClosestStar(pez.position, estrellas);
    // Calcular fuerza para ir hacia la estrella
    let desired = p5.Vector.sub(target.position, pez.position);
    desired.setMag(0.5); // fuerza constante hacia la estrella

    pez.applyForce(desired);

    // Guardar dirección para rotar pez hacia la estrella
    pez.heading = desired.heading();
  } else {
    // Si no hay estrella, que mire hacia donde va (o no gire)
    if (pez.velocity.mag() > 0.1) {
      pez.heading = pez.velocity.heading();
    }
  }

  // Aplicar resistencia de fluidos
  let fluidDensity = 0.1;
  let drag = pez.velocity.copy();
  drag.normalize();
  drag.mult(-1);
  let speedSq = pez.velocity.magSq();
  drag.setMag(fluidDensity * speedSq);
  pez.applyForce(drag);

  pez.update();
  pez.edges();
  pez.show();
}

// Crear estrellas con click
function mousePressed() {
  let star = new Estrella(random(50, width - 50), random(50, height - 50));
  estrellas.push(star);
}

// Regresa la estrella más cercana a una posición
function getClosestStar(pos, stars) {
  let record = Infinity;
  let closest = null;
  for (let star of stars) {
    let d = p5.Vector.dist(pos, star.position);
    if (d < record) {
      record = d;
      closest = star;
    }
  }
  return closest;
}

// Clase Mover para el pez
class Mover {
  constructor(x, y, m) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = m;
    this.r = this.mass * 8;
    this.heading = 0; // Dirección del pez (radianes)
  }

  applyForce(force) {
    // F = m * a => a = F / m
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  edges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -0.5;
    } else if (this.position.x < 0) {
      this.position.x = 0;
      this.velocity.x *= -0.5;
    }

    if (this.position.y > height) {
      this.position.y = height;
      this.velocity.y *= -0.5;
    } else if (this.position.y < 0) {
      this.position.y = 0;
      this.velocity.y *= -0.5;
    }
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.heading);
    noStroke();
    fill(255, 100, 100);
    // Cuerpo (elipse centrada)
    ellipse(0, 0, this.r * 1.5, this.r);
    // Cola (triángulo hacia atrás)
    triangle(-this.r * 0.75, 0, -this.r * 1.3, -this.r * 0.5, -this.r * 1.3, this.r * 0.5);
    pop();
  }
}

// Clase para la estrella de mar
class Estrella {
  constructor(x, y) {
    this.position = createVector(x, y);
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    fill(255, 204, 0);
    noStroke();
    // Dibujar estrella de mar simple
    beginShape();
    for (let i = 0; i < 5; i++) {
      let angle = TWO_PI / 5 * i - HALF_PI;
      let x = cos(angle) * 15;
      let y = sin(angle) * 15;
      vertex(x, y);
      angle += TWO_PI / 10;
      vertex(cos(angle) * 7, sin(angle) * 7);
    }
    endShape(CLOSE);
    pop();
  }
}
```

 **Imagen:**

<img width="637" height="397" alt="image" src="https://github.com/user-attachments/assets/72e23c83-bd3a-4963-9040-d9ae7091acc3" />

- **Atracción gravitacional:**
  - **Explica cómo modelaste cada fuerza:**
  La fuerza se modeló con la ley de atracción gravitacional, calculando un vector dirigido de cada pez hacia la fosa.

    
  - **Conceptualmente cómo se relaciona la fuerza con la obra generativa:**
   En la obra, esta fuerza representa la atracción de la fosa y provoca que los peces sean absorbidos, creando el patrón dinámico y estético del flujo hacia el centro

  **Enlace:** https://editor.p5js.org/ZombieYuu/sketches/R6LJ4I94M
  
  **Código:**

```javascript

// Generative Art: Fosa gravitacional + peces
// Basado en conceptos de The Nature of Code (Unidad 1-2: Vectores y Fuerzas)

let fishes = [];
let G = 0.8;            // Constante gravitacional
let pitRadius = 120;    // Radio visual máximo de la fosa (para el gradiente)
let consumeRadius = 18; // Radio de "consumo" (desaparición de peces)

// Paleta
const bgColor = [190, 225, 255];     // azul claro del lienzo
const deepBlue = [0, 30, 120];       // azul intenso del centro de la fosa

function setup() {
  createCanvas(800, 600);
  for (let i = 0; i < 80; i++) {
    fishes.push(new Fish());
  }
}

function draw() {
  background(bgColor[0], bgColor[1], bgColor[2]);

  // Dibujar fosa en el mouse con gradiente azul->fondo
  drawPit(mouseX, mouseY, pitRadius);

  for (let i = fishes.length - 1; i >= 0; i--) {
    const f = fishes[i];

    // Atracción gravitacional hacia el mouse
    const force = attract(createVector(mouseX, mouseY), f.pos, f.mass);
    f.applyForce(force);
    f.update();
    f.edges();

    const d = p5.Vector.dist(f.pos, createVector(mouseX, mouseY));
    if (d < consumeRadius) {
      fishes.splice(i, 1);
      fishes.push(new Fish(true));
      continue;
    }

    f.show();
  }
}

// --- Ley de atracción gravitacional ---
function attract(attractorPos, objectPos, mObject) {
  let dir = p5.Vector.sub(attractorPos, objectPos);
  let r = dir.mag();
  r = constrain(r, 8, 200);
  dir.normalize();
  const m1 = 20;
  const strength = (G * m1 * mObject) / (r * r);
  dir.mult(strength);
  return dir;
}

// --- Clase Fish ---
class Fish {
  constructor(spawnAtEdge = false) {
    this.mass = random(0.6, 2.0);
    if (spawnAtEdge) {
      this.pos = this.randomEdgePosition();
    } else {
      this.pos = createVector(random(width), random(height));
    }
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);

    this.col = color(random(60, 255), random(60, 255), random(60, 255), 220);
  }

  randomEdgePosition() {
    const side = floor(random(4));
    if (side === 0) return createVector(random(width), -10);
    if (side === 1) return createVector(width + 10, random(height));
    if (side === 2) return createVector(random(width), height + 10);
    return createVector(-10, random(height));
  }

  applyForce(force) {
    const f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(6);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  edges() {
    const margin = 40;
    if (this.pos.x < -margin || this.pos.x > width + margin ||
        this.pos.y < -margin || this.pos.y > height + margin) {
      const newPos = this.randomEdgePosition();
      this.pos.set(newPos.x, newPos.y);
      this.vel.set(0, 0);
    }
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.vel.heading());

    noStroke();
    fill(this.col);

    const body = 8 * this.mass;
    const tail = 12 * this.mass;

    beginShape();
    vertex(body, 0);
    vertex(-tail * 0.6, body * 0.6);
    vertex(-tail * 0.6, -body * 0.6);
    endShape(CLOSE);

    fill(255, 230);
    circle(body * 0.5, -body * 0.2, 2.2 * this.mass);

    pop();
  }
}

// --- Dibujo del pozo con gradiente azul -> azul claro ---
function drawPit(x, y, rMax) {
  noStroke();
  for (let r = rMax; r > 0; r -= 2) {
    const t = r / rMax; // 1 = borde, 0 = centro
    // Interpolar entre azul profundo (centro) y azul claro (fondo)
    const rr = lerp(deepBlue[0], bgColor[0], t);
    const gg = lerp(deepBlue[1], bgColor[1], t);
    const bb = lerp(deepBlue[2], bgColor[2], t);
    fill(rr, gg, bb, 255);
    ellipse(x, y, r * 2, r * 2);
  }
}

```

**Imagen:** 

<img width="938" height="745" alt="image" src="https://github.com/user-attachments/assets/7e00d72d-5280-418d-bcfe-fd5b51c77955" />
