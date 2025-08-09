# Unidad 2


## 🛠 Fase: Apply

### Actividad 8: 

1. **Describe el concepto de tu obra generativa.**

   - Quiero hacer que unas partículas en forma de gusano se muevan utililando motiom 101 conbinado con levy flyght y que a su vex huyan del mouse cuando se acerca.
     
2. **¿Cómo piensas aplicar el marco MOTION 101 y por qué?**

   - Se aplica el motion 101 cuando se le aplica una aceleracion a una velocidad luego esta se restringe a una velocidad máxima y se aplica esta a una posición.
  
3. **¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?**

   - Una aceleración que depende del Levy flyght y de la fuerza de repulción, la cual es variable. 

4. **El contenido generado debe ser interactivo. Puedes utilizar mouse, teclado, cámara, micrófono, etc, para variar los parámetros del algoritmo en tiempo real.**

   - Si, es intersctivo en tiempo real pues la obra interactúa con el mouse de manera que los gusanitos se alejen de el.
 
5. **El código de la aplicación.**

```javascript
let agents = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  for (let i = 0; i < 150; i++) {
    agents.push(new Agent(random(width), random(height)));
  }
  background(0);
}

function draw() {
  fill(0, 10); // efecto de rastro suave
  noStroke();
  rect(0, 0, width, height);

  for (let agent of agents) {
    agent.update();
    agent.show();
  }
}

class Agent {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.maxSpeed = 3;
    this.color = color(random(100, 255), random(100, 255), random(100, 255));
  }

  applyForce(force) {
    this.acc.add(force);
  }

  levyFlight() {
    // Levy flight con paso aleatorio largo ocasional
    let angle = random(TWO_PI);
    let step = pow(random(1), -1.5); // Distribución de Levy
    let stepVec = p5.Vector.fromAngle(angle).mult(step * 0.5); // escala el paso
    this.applyForce(stepVec);
  }

  interactWithMouse() {
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(this.pos, mouse); // dirección desde el mouse hacia el agente
    let d = dir.mag();
    d = constrain(d, 5, 200); // evita divisiones por 0 y limita la fuerza máxima
    dir.normalize();

    let force = 20 / (d * d); // repulsión fuerte si está cerca
    dir.mult(force);

    this.applyForce(dir);
  }

  update() {
    this.levyFlight();         // Movimiento aleatorio tipo Levy
    this.interactWithMouse();  // Repulsión al mouse

    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed); // Limita velocidad máxima
    this.pos.add(this.vel);
    this.acc.mult(0); // Reset aceleración

    // Rebote en los bordes
    if (this.pos.x > width || this.pos.x < 0) this.vel.x *= -1;
    if (this.pos.y > height || this.pos.y < 0) this.vel.y *= -1;
  }

  show() {
    stroke(this.color);
    strokeWeight(2);
    point(this.pos.x, this.pos.y);
  }
}
```
   
6. **Un enlace al proyecto en el editor de p5.js.**

https://editor.p5js.org/ZombieYuu/sketches/GRz0fnxzZ
   
7. **Una captura de pantalla representativa de tu pieza de arte generativo.**

https://github.com/user-attachments/assets/e3d2dbe2-49cf-4981-8864-e10b1ce44df3

