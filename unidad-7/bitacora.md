# Evidencias de la unidad 7

## Actividad 1: Set

**Exploración de ejemplos:**

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/9bf6848c-354a-42f6-ba38-f3d6fc7d5255" />

Me gusta que de manera minimalista se da a entender la acción del sol poniendose, se ve muy elegante, haciendo qy¿ue ver la interacción lo haga mas entretenido e interesante.

<img width="500" height="499" alt="image" src="https://github.com/user-attachments/assets/8f77f975-5907-49cd-925c-438ffe7ac62d" />

Esta me parece muy graciosa porque de una manera cómica da a entender que significa la palabra.

<img width="500" height="501" alt="image" src="https://github.com/user-attachments/assets/29bac9e3-2b60-4627-afc5-9a929cb9b852" />

Me parece muy bonito como utilizan la c para hacer simular el eclipce como se ve en la vida real.

**Ideas:**

- Con la palabra *amor* quiero genrar un corazón con la o y que palpite.
- Con la palabra *mar* quiero que llegue una ola y se lleve las letras.
- Con la palabra *animación* volver la letra A en una persona caminando.

## Actividad 2: Seek

### Experimeto:

En este experimento utilicé el ``MouseConstraint``, un mundo con gravedad e incluí dos objetos simples como un triangulo y un círculo.

**Código**

``` javascript
// Importar módulos de Matter.js
const { Engine, Render, World, Bodies, Body, Mouse, MouseConstraint, Composite, Vertices } = Matter;

let engine, world;
let ground;
let triangle, circleBody;
let mConstraint;

function setup() {
  createCanvas(600, 400);
  
  // Crear motor físico
  engine = Engine.create();
  world = engine.world;
  engine.gravity.y = 1; // gravedad
  
  // Crear suelo estático
  let groundOptions = {
    isStatic: true
  };
  ground = Bodies.rectangle(width / 2, height - 20, width, 40, groundOptions);
  World.add(world, ground);
  
  // Crear triángulo
  let triangleVertices = Vertices.fromPath('0 0 40 80 -40 80');
  triangle = Bodies.fromVertices(200, 100, triangleVertices, {
    restitution: 0.5,
    friction: 0.3
  });
  World.add(world, triangle);
  
  // Crear círculo
  circleBody = Bodies.circle(400, 100, 30, {
    restitution: 0.5,
    friction: 0.3
  });
  World.add(world, circleBody);
  
  // Crear control con mouse
  const canvasMouse = Mouse.create(canvas.elt);
  const mouseParams = {
    mouse: canvasMouse,
    constraint: {
      stiffness: 0.2,
      render: { visible: false }
    }
  };
  mConstraint = MouseConstraint.create(engine, mouseParams);
  World.add(world, mConstraint);
}

function draw() {
  background(200);
  Engine.update(engine);

  // Dibujar suelo
  noStroke();
  fill(100);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 40);

  // Dibujar triángulo
  drawVertices(triangle.vertices, color(0, 150, 255));

  // Dibujar círculo
  fill(255, 100, 100);
  ellipse(circleBody.position.x, circleBody.position.y, 60);

  // Opcional: dibujar línea de mouse si se está arrastrando algo
  if (mConstraint.body) {
    let pos = mConstraint.body.position;
    let m = mConstraint.mouse.position;
    stroke(0);
    line(pos.x, pos.y, m.x, m.y);
  }
}

// Función auxiliar para dibujar el triángulo
function drawVertices(vertices, col) {
  fill(col);
  beginShape();
  for (let i = 0; i < vertices.length; i++) {
    vertex(vertices[i].x, vertices[i].y);
  }
  endShape(CLOSE);
}
```


**Captura y enlace** 



https://github.com/user-attachments/assets/d1dbb1dc-37f5-4f37-9c67-74e04419eff8

https://editor.p5js.org/ZombieYuu/sketches/Wsthoxogp

**Explicación de conceptos:** 

- ``Engine`` Es el motoor de la simulación de físicas.
- ``World`` Es el lugar donde ocurre la simulación.
- ``Bodies`` Es el nombre para crear tipos específicos de cuerpos con distintas características y unas propiedades físicas predeterminadas.
- ``Constraint`` Es una conexión entre varios cuerpos.
- ``MouseConstraint`` Es una conexión con el mouse.

## Actividad 3: Apply  


**Idea:**

La palabra que voy a utilizar es *mar* y quero que llegue una ola y se lleve las letras.

**Aspectos técnicos:**

**1. Formación de las letras con Matter.js**
Cada letra de la palabra *"MAR"* se representa mediante un **cuerpo físico rectangular** de Matter.js (`Bodies.rectangle`) que actúa como un bloque rígido.
Aunque visualmente se muestra con texto (`text(l.char, 0, 0)`), el cuerpo invisible es el que tiene las propiedades físicas y colisiona con el entorno.

**2. Propiedades físicas utilizadas**

* **`frictionAir` (0.01):** controla la resistencia del aire, permitiendo que las letras se muevan libremente cuando el tsunami las empuja.
* **`restitution` (0.5):** da rebote a los cuerpos, aportando dinamismo al impacto con el suelo y las olas.
* **`density` (0.004):** ajusta la masa de las letras; una densidad baja hace que puedan ser arrastradas más fácilmente por las fuerzas del “agua”.
* **`gravity.y` (0.5):** se usa para simular la caída natural y el efecto de peso.

**3. Fuerzas y comportamiento del tsunami**
La ola está compuesta por **partículas visuales** que no tienen colisiones físicas, pero generan la ilusión del agua.
El movimiento de la ola se traduce en **fuerzas aplicadas directamente** sobre las letras (`Body.applyForce`), empujándolas con intensidad variable en el eje X y ligeramente hacia arriba en el eje Y.
Esto crea la sensación de que la ola las **levanta, arrastra y las lanza fuera del lienzo**.

**4. Restricciones**
No se usaron **restricciones (`Constraint`)** en este caso, porque se buscaba que las letras se comportaran como objetos completamente libres, sin estar conectados entre sí ni al entorno.
Esto refuerza el concepto simbólico de que el mar rompe toda estructura y se lleva todo lo que toca.

**Código:*

``` JavaScript

let Engine = Matter.Engine,
  World = Matter.World,
  Bodies = Matter.Bodies,
  Body = Matter.Body;

let engine, world;
let ground;
let letters = [];
let particles = [];
let waveX = -700;
let waveSpeed = 12; // ola devastadora
let tsunamiForce = 0.035; // fuerza brutal
let sceneBlue = 180;
let tsunamiArrived = false;
let shakeIntensity = 0; // vibración para dramatismo

function setup() {
  createCanvas(900, 500);
  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 0.5;

  // suelo
  ground = Bodies.rectangle(width / 2, height - 10, width, 20, { isStatic: true });
  World.add(world, ground);

  createLetters();
}

function draw() {
  // efecto de temblor
  if (shakeIntensity > 0) {
    translate(random(-shakeIntensity, shakeIntensity), random(-shakeIntensity, shakeIntensity));
    shakeIntensity *= 0.95;
  }

  background(sceneBlue, 230, 255);
  Engine.update(engine);

  // arena
  noStroke();
  fill("#d4c19c");
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 20);

  // tsunami avanza
  waveX += waveSpeed;

  // partículas del tsunami
  generateWaveParticles(waveX);

  for (let p of particles) {
    p.update();
    p.display();
  }
  particles = particles.filter(p => !p.isDead());

  // aplicar fuerza destructiva
  applyTsunamiForce();

  // dibujar letras
  textAlign(CENTER, CENTER);
  textSize(110);
  fill(0);
  for (let l of letters) {
    push();
    translate(l.body.position.x, l.body.position.y);
    rotate(l.body.angle);
    text(l.char, 0, 0);
    pop();
  }

  // texto poético
  fill(0, 150);
  textSize(18);
  textAlign(LEFT);
  text("La fuerza del mar", 20, 30);

  // cuando todas las letras desaparecen del lienzo
  if (letters.every(l => l.body.position.x > width + 400)) {
    tsunamiArrived = true;
  }

  if (tsunamiArrived) {
    sceneBlue = lerp(sceneBlue, 20, 0.03);
    fill(255, 240);
    textSize(22);
    textAlign(CENTER);
    text("Solo queda el mar infinito.", width / 2, height / 2);
  }
}

function createLetters() {
  const chars = ["M", "A", "R"];
  const startX = width / 2 - 160;
  for (let i = 0; i < chars.length; i++) {
    let x = startX + i * 150;
    let y = height - 140;
    let body = Bodies.rectangle(x, y, 90, 100, {
      frictionAir: 0.01,
      restitution: 0.5,
      density: 0.004,
    });
    World.add(world, body);
    letters.push({ char: chars[i], body });
  }
}

function applyTsunamiForce() {
  let waveFront = waveX;
  let waveWidth = 600; // ola masiva
  let waveHeight = height - 120 + sin(frameCount * 0.02) * 150; // cresta alta

  // temblor cuando el tsunami entra
  if (waveFront < width / 3) shakeIntensity = 3;
  else if (waveFront < width / 2) shakeIntensity = 6;
  else if (waveFront < (width * 2) / 3) shakeIntensity = 10;

  for (let l of letters) {
    let b = l.body;
    let pos = b.position;

    // fuerza horizontal destructiva
    if (pos.x > waveFront - waveWidth / 2 && pos.x < waveFront + waveWidth) {
      let power = map(waveFront - pos.x, -waveWidth, waveWidth, 0.015, tsunamiForce);
      Body.applyForce(b, pos, { x: power, y: -0.004 }); // empuje brutal y ascendente
    }

    // flotación turbulenta dentro del agua
    if (pos.y > waveHeight - 80 && pos.y < height - 10) {
      Body.applyForce(b, pos, { x: random(-0.002, 0.002), y: -0.004 });
    }
  }
}

function generateWaveParticles(frontX) {
  let yBase = height - 120 + sin(frameCount * 0.03) * 100;
  for (let i = 0; i < 60; i++) {
    let x = frontX + random(-150, 150);
    let y = yBase + random(-80, 80);
    let colorChoice =
      random() < 0.5
        ? color(255, 255, 255, random(200, 255)) // espuma blanca
        : color(20, random(130, 200), 255, random(230, 255)); // azul brillante
    particles.push(new Particle(x, y, random(4, 9), colorChoice));
  }
}

class Particle {
  constructor(x, y, r, col) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-3, 7), random(-3, 3));
    this.acc = createVector(0.1, 0.08);
    this.r = r;
    this.lifespan = 255;
    this.col = col;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.lifespan -= 3;
  }

  display() {
    noStroke();
    fill(red(this.col), green(this.col), blue(this.col), this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }

  isDead() {
    return this.pos.x > width + 400 || this.lifespan < 0;
  }
}
```

**Animación** 



https://github.com/user-attachments/assets/2ffb1915-b2af-4e84-bd2c-2e46aa747746

https://editor.p5js.org/ZombieYuu/sketches/J-Qz04YTW

## Autoevaluación:

Mi auto evaluación es de 5.0, pues completé todas las actividades y entendí los conceptos.




