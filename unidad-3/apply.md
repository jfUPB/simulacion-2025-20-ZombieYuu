# Unidad 3


## 🛠 Fase: Apply

### Explica cómo modelaste el problema de los n-cuerpos en tu obra:

Mi obra consta de un cardumen de peces que se atraen ente  sí y son atraídos por el mouse como un imán, también utilicé los conceptos de random, distribución normal y de probabilidad.

En esta simulación se modeló el problema de n cuerpos usando una aproximación simplificada basada en física newtoniana. Cada pez actúa como un cuerpo con masa que ejerce una fuerza de atracción sobre los demás, similar a la gravedad. Además, el mouse representa un atractor dominante con gran masa, lo que genera un comportamiento colectivo interesante donde los cuerpos se agrupan y se sienten atraídos dinámicamente hacia él. El sistema visualiza cómo las interacciones múltiples influyen en el movimiento de cada elemento del conjunto.

### Relación con las obras de Alexander Calder

| *Elemento en Calder*            | *Cómo se refleja en tu obra generativa*                                                                                         |
| --------------------------------| ------------------------------------------------------------------------------------------------------------------------------  |
| *Movimiento orgánico*           | Los movers (peces) se desplazan en trayectorias fluidas y cambiantes gracias a las fuerzas de atracción.                        |
| *Interacción con el espectador* | El mouse funciona como “corriente de aire” que reorganiza el movimiento, igual que la interacción del público con los móviles.  |
| *Equilibrio dinámico*           | La atracción entre peces y hacia el mouse crea un sistema en constante ajuste, nunca estático pero siempre balanceado.          |
| *Colores y formas abstractas*   | Los peces se representan con curvas, triángulos y colores vibrantes gaussianos, recordando la estética abstracta de Calder.     |
| *Inspiración en la naturaleza*  | El cardumen digital simboliza un ecosistema vivo, paralelo a cómo Calder evocaba el cosmos o sistemas biológicos en sus móviles.|

### Copia el enlace a tu simulación en p5.js:
https://editor.p5js.org/ZombieYuu/sketches/OUwVulQe-

### Código:

``` javascript
let movers = [];
let NUM_FISH = 50;

function setup() {
  createCanvas(800, 600);
  for (let i = 0; i < NUM_FISH; i++) {
    let angle = random(TWO_PI);
    let radius = random(100, 250);
    let pos = p5.Vector.fromAngle(angle).mult(radius);
    let vel = createVector(0, 0); // Sin velocidad inicial
    let mass = randomGaussian(10, 3);
    movers[i] = new Mover(pos.x, pos.y, vel.x, vel.y, mass);
  }
  background(0, 30, 60);
}

function draw() {
  background(0, 30, 60, 25); // Estela
  translate(width / 2, height / 2);

  let mouse = createVector(mouseX - width / 2, mouseY - height / 2);
  let mouseMass = 1000; // Aumentamos mucho la masa del mouse para más atracción

  for (let mover of movers) {
    // Atracción al mouse
    let force = p5.Vector.sub(mouse, mover.pos);
    let distance = constrain(force.mag(), 10, 300);
    force.normalize();
    let strength = (mouseMass * mover.mass) / (distance * distance);
    force.mult(strength * 0.05); // Multiplicador mayor para más fuerza
    mover.applyForce(force);

    // Atracción entre peces (puedes comentar esta parte si no la quieres)
    for (let other of movers) {
      if (mover !== other) {
        mover.attract(other);
      }
    }

    mover.update();
    mover.showFish();
  }
}

class Mover {
  constructor(x, y, vx, vy, m) {
    this.pos = createVector(x, y);
    this.vel = createVector(vx, vy);
    this.acc = createVector(0, 0);
    this.mass = constrain(m, 5, 20);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  attract(other) {
    let force = p5.Vector.sub(this.pos, other.pos);
    let distance = constrain(force.mag(), 5, 50);
    force.normalize();
    let strength = (this.mass * other.mass) / (distance * distance);
    force.mult(strength * 0.001); // Fuerza muy reducida entre peces
    other.applyForce(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  showFish() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.vel.heading());

    let fishLength = this.mass * 2;
    let fishHeight = this.mass;

    let r = constrain(randomGaussian(150, 50), 0, 255);
    let g = constrain(randomGaussian(100, 50), 0, 255);
    let b = constrain(randomGaussian(200, 30), 0, 255);
    fill(r, g, b, 180);
    noStroke();

    beginShape();
    vertex(-fishLength / 2, 0);
    bezierVertex(-fishLength / 4, -fishHeight, fishLength / 4, -fishHeight, fishLength / 2, 0);
    bezierVertex(fishLength / 4, fishHeight, -fishLength / 4, fishHeight, -fishLength / 2, 0);
    endShape(CLOSE);

    fill(255, 100);
    triangle(-fishLength / 2, 0, -fishLength / 1.5, -fishHeight / 2, -fishLength / 1.5, fishHeight / 2);

    pop();
  }
}
```

### Captura una imagen representativa de tu ejemplo:

<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/8532666e-c920-449e-8c33-79887adc2c21" />

