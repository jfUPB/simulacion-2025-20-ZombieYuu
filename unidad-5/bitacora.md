# Evidencias de la unidad 5


## Actividad 2:

### Ejemplo 4.2:  an Array of Particles

- Con la siguiente instancia `` particles.push(new Particle(width / 2, 20));`` se crea cada partícula por cada frame.
- La eliminación de partículas se elabora al "revés" pues se "matan" las partículas mas viejas.
- La memoria se gestiona por medio de un ``array`` con el método de ``run`` lo que genera un loop en el ``arraylist`` y elimina las partículas muertas.

  ### Obra generativa 1:
  - En la grstión de creación y desaparición de partículas, creo partículas nuevas, las dejo vivir un tiempo, y cuando mueren las borro de la lista para que no ocupen memoria.
  - En la base de *Array of particles* utilicé los conceptos de distribución no uniforme y el random de la unidad 1 para cambiar el color de las partículas con la tecla *c*. Generando así una simulación de juegos artificiales.
  - https://editor.p5js.org/ZombieYuu/sketches/kPezPX3sh
 
  - Código:
```js 
let particles = [];
let emitter;
let currentColor;
let palettes;
let paletteIndex = 0;


let modeCount = 5;            
let modeAngles = [];         
let modeWeights = [];        
let spread = 0.6;            

let emissionRate = 8;        
let maxParticles = 2000;     

function setup() {
  createCanvas(900, 900);
  pixelDensity(1);
  emitter = createVector(width / 2, height / 2);
  background(20);

  // paletas de ejemplo
  palettes = [
    ['#FF6B6B', '#FFD93D', '#6BCB77', '#4D96FF'],
    ['#F72585', '#B5179E', '#7209B7', '#3A0CA3'],
    ['#06D6A0', '#118AB2', '#073B4C', '#FFD166'],
    ['#FFADAD', '#FFD6A5', '#FDFFB6', '#CAFFBF'],
  ];
  currentColor = color(random(palettes[paletteIndex]));

  // inicializar modes con ángulos distribuidos, pero con pesos aleatorios
  for (let i = 0; i < modeCount; i++) {
    modeAngles.push(random(TWO_PI));
    // pesos que favorecen algunos modos: exponencial para acentuar desigualdad
    modeWeights.push(pow(random(), 2) + 0.05);
  }
  // normalizar pesos
  let sum = modeWeights.reduce((a, b) => a + b, 0);
  modeWeights = modeWeights.map(w => w / sum);

  noStroke();
}

function draw() {
  // fondo con alpha para estelas
  fill(20, 20, 20, 35);
  rect(0, 0, width, height);

  // emitir partículas
  for (let i = 0; i < emissionRate; i++) {
    if (particles.length < maxParticles) particles.push(new Particle(emitter.x, emitter.y, currentColor));
  }

  // actualizar y mostrar
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.update();
    p.display();
    if (p.isDead()) particles.splice(i, 1);
  }

  // indicador del emisor
  push();
  fill(255, 200);
  stroke(255, 120);
  strokeWeight(1);
  ellipse(emitter.x, emitter.y, 6, 6);
  pop();

  // info mínima en pantalla
  push();
  noStroke();
  fill(255, 180);
  textSize(12);
  textAlign(LEFT, BOTTOM);
  text("Partículas: " + particles.length + "  |  Presiona 'c' para cambiar color de nuevas partículas", 10, height - 10);
  pop();
}

// sampleAngle genera ángulos no uniformes: mezcla de lobos (modes)
function sampleAngle() {
  // elegir un modo según modeWeights
  let r = random();
  let acc = 0;
  let chosen = 0;
  for (let i = 0; i < modeCount; i++) {
    acc += modeWeights[i];
    if (r <= acc) {
      chosen = i;
      break;
    }
  }
  // ángulo centrado en modeAngles[chosen] con jitter gaussiano
  let theta = modeAngles[chosen] + randomGaussian() * spread;
  // normalizar
  return theta % (TWO_PI);
}

class Particle {
  constructor(x, y, c) {
    this.pos = createVector(x, y);
    // velocidad: dirección no uniforme + magnitud variable
    let angle = sampleAngle();
    let speed = random(1.2, 5.6);
    this.vel = p5.Vector.fromAngle(angle).mult(speed);

    // pequeña variación para bi-dimensionalidad
    this.acc = createVector(random(-0.02, 0.02), random(-0.02, 0.02));

    this.lifespan = random(160, 380);
    this.size = random(2, 7);

    // color copiado (las partículas existentes no cambian cuando presionas 'c')
    this.color = color(c);

    // darles un poco de 'rotation' visual
    this.rotation = random(TWO_PI);
    this.rotationSpeed = random(-0.03, 0.03);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.lifespan -= 1.0;
    this.rotation += this.rotationSpeed;

  }

  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.rotation);
    let alpha = map(this.lifespan, 0, 380, 0, 255);
    fill(red(this.color), green(this.color), blue(this.color), alpha);
    noStroke();
    ellipse(0, 0, this.size, this.size);
    pop();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

function keyPressed() {
  if (key === 'c' || key === 'C') {
    // cambiar índice de paleta y el color actual para nuevas partículas
    paletteIndex = (paletteIndex + 1) % palettes.length;
    let pal = palettes[paletteIndex];
    currentColor = color(random(pal));

    // opcional: cambiar también los modos direccionales para variar la distribución
    jitterModes();
  }
}

function jitterModes() {
  // mueven ligeramente los centros de los lobos y recalculan pesos
  for (let i = 0; i < modeCount; i++) {
    modeAngles[i] = (modeAngles[i] + random(-0.6, 0.6)) % TWO_PI;
    modeWeights[i] = constrain(modeWeights[i] + random(-0.08, 0.08), 0.01, 1);
  }
  // renormalizar
  let sum = modeWeights.reduce((a, b) => a + b, 0);
  modeWeights = modeWeights.map(w => w / sum);
}

// Opciones de interacción extra:
// - Mouse arrastra el emisor
function mouseDragged() {
  emitter.x = mouseX;
  emitter.y = mouseY;
}

function windowResized() {
  // si prefieres mantener el tamaño fijo, comenta esto
  resizeCanvas(900, 900);
}
``` 

 <img width="898" height="773" alt="image" src="https://github.com/user-attachments/assets/79d31e31-45cd-4ffe-a50f-529ee683e796" />


### Ejemplo 4.4: A system of systems

- Se generan varios sistemas de partículas de forma independiente.
- Se matan las partículas mas viejas de cada sistema.
- Se crea un ``array`` para cada sistema para eliminar las partículas muertas.

  ### Obra generativa 2:
  - La gestión de partículas es diferente, al inicio se crean las partículas normalmente, luego las viejas se guardan en ``pool``, y cuando necesito nuevas las reciclo de ahí y cuando hay demaciadas partículas no permito que se creen mas.
  - Tomando como base *A system of systems* utilicé el concepto de los N Cuerpos de la unidad 3 y el random de la unidad 1 para crear la obra que sigue el mouse como punto gravitacional despues de ser generado.
  - https://editor.p5js.org/ZombieYuu/sketches/AC5QhH7P1
  - Código:
``` js


let systems = [];
let particlePool = [];
let activeParticles = [];
const MAX_PARTICLES = 600; // límite total para mantener la performance
const G = 60;              // "constante gravitatoria" ajustable
const SOFTENING = 8;      // evita singularidades (suavizado)
const MAX_INTERACTIONS = 1200; // para evitar O(n^2) completo en partículas grandes (cap)

function setup() {
  createCanvas(1080, 720);
  pixelDensity(1);
  noStroke();
  // preparar pool inicial (vacío — se rellenará bajo demanda)
}

function draw() {
  // ligeramente opaco para estela suave
  background(10, 12, 16, 20);

  // actualizar y dibujar cada sistema (emisor)
  for (let sys of systems) {
    sys.update();
    sys.display();
  }

  // actualizar partículas (fuerzas entre partículas: N-body aproximado)
  applyNBodyForces();

  // actualizar posiciones y dibujar partículas activas
  for (let i = activeParticles.length - 1; i >= 0; i--) {
    const p = activeParticles[i];
    p.update();
    p.display();
    if (p.dead) {
      recycleParticle(i);
    }
  }

  // opcional: limpiar sistemas inactivos
  for (let i = systems.length - 1; i >= 0; i--) {
    if (systems[i].finished && systems[i].particleCount === 0) {
      systems.splice(i, 1);
    }
  }

  // UI mínima en pantalla
  fill(255, 200);
  textSize(12);
  text(`Sistemas: ${systems.length}   Partículas: ${activeParticles.length}`, 10, height - 10);
}

// Crear un emisor en forma de cascada al hacer click
function mousePressed() {
  // posición del emisor = mouse; puedes alternar arriba-center si quieres cascada desde arriba
  let sys = new ParticleSystem(createVector(mouseX, mouseY));
  systems.push(sys);
  // disparo inicial (burst) para simular cascada de inicio
  sys.startBurst();
}

// -------------------- ParticleSystem (System of Systems) --------------------
class ParticleSystem {
  constructor(pos) {
    this.pos = pos.copy();
    this.particlesToEmit = 120; // cantidad total que emitirá en su vida (cascada)
    this.emitRate = 8;         // partículas por frame mientras haya pendientes
    this.particleCount = 0;    // activas generadas por este sistema
    this.age = 0;
    this.finished = false;
    // color base por sistema (varía sutilmente)
    this.hue = random(160, 320);
  }

  startBurst() {
    // Emite un poco extra al inicio para dar efecto cascada inmediato
    for (let i = 0; i < 30; i++) this.emitOne();
  }

  emitOne() {
    if (totalActiveParticles() >= MAX_PARTICLES) return;
    if (this.particlesToEmit <= 0) return;

    // posición de emisión: ligera variación para sensación de cascada
    let px = this.pos.x + random(-18, 18);
    let py = this.pos.y + random(-6, 6);

    let p = obtainParticle(px, py);
    // velocidad inicial: hacia abajo con dispersión — forma de cascada
    let angle = random(PI/6, (5*PI)/6); // mayormente hacia abajo
    let speed = random(1.2, 4.0);
    p.pos.set(px, py);
    p.vel.set(cos(angle) * speed + random(-0.6,0.6), sin(angle) * speed + random(-0.6,0.6));
    p.acc.set(0, 0);
    p.mass = random(0.6, 2.8);
    p.lifetime = int(random(90, 260));
    p.size = map(p.mass, 0.6, 2.8, 2.2, 6.2);
    p.color = colorFromHue(this.hue + random(-24, 24));
    p.system = this;
    p.dead = false;

    this.particlesToEmit--;
    this.particleCount++;
    activeParticles.push(p);
  }

  update() {
    this.age++;
    // emitir de forma gradual para cascada
    for (let i = 0; i < this.emitRate; i++) {
      if (this.particlesToEmit > 0) this.emitOne();
    }
    if (this.particlesToEmit <= 0) this.finished = true;
  }

  display() {
    // Opcional: dibujar un sutil marcador del emisor
    push();
    noStroke();
    fill(255, 12);
    ellipse(this.pos.x, this.pos.y, 6, 6);
    pop();
  }
}

// -------------------- Particle & Pooling --------------------
class Particle {
  constructor() {
    this.pos = createVector();
    this.vel = createVector();
    this.acc = createVector();
    this.mass = 1;
    this.size = 3;
    this.lifetime = 120;
    this.age = 0;
    this.color = color(255);
    this.system = null;
    this.dead = true;
  }

  applyForce(f) {
    // F = m * a -> a += f / m
    let a = p5.Vector.div(f, this.mass);
    this.acc.add(a);
  }

  update() {
    this.age++;
    // attracción al mouse (global)
    if (mouseIsPressed || mouseX >= 0) {
      let mousePos = createVector(constrain(mouseX, 0, width), constrain(mouseY, 0, height));
      let dir = p5.Vector.sub(mousePos, this.pos);
      let d2 = constrain(dir.magSq(), 25, 20000);
      let strength = (G * 2 * this.mass) / d2; // fuerza proporcional
      dir.normalize();
      dir.mult(strength);
      this.applyForce(dir);
    }

    // Integración simple
    this.vel.add(this.acc);
    // limit velocity to avoid blowups
    this.vel.limit(12);
    this.pos.add(this.vel);
    this.acc.set(0, 0);

    // envejecimiento y muerte por límites
    if (this.age > this.lifetime) this.dead = true;
    if (this.pos.x < -50 || this.pos.x > width + 50 || this.pos.y < -50 || this.pos.y > height + 200) {
      // permitir caída fuera del canvas un poco
      this.dead = true;
    }
  }

  display() {
    push();
    translate(this.pos.x, this.pos.y);
    // brillo sutil según velocidad
    let alpha = map(this.lifetime - this.age, 0, this.lifetime, 12, 220);
    fill(red(this.color), green(this.color), blue(this.color), alpha);
    noStroke();
    ellipse(0, 0, this.size, this.size);
    pop();
  }

  reset() {
    this.pos.set(-9999, -9999);
    this.vel.set(0, 0);
    this.acc.set(0, 0);
    this.mass = 1;
    this.size = 3;
    this.lifetime = 0;
    this.age = 0;
    this.color = color(255);
    this.system = null;
    this.dead = true;
  }
}

function obtainParticle(x=0,y=0) {
  // reutiliza objeto si hay en pool, sino crea uno nuevo (si no excede MAX)
  if (particlePool.length > 0) {
    let p = particlePool.pop();
    p.reset();
    p.pos.set(x, y);
    return p;
  } else {
    // crear nuevo solo si no excedemos MAX_PARTICLES
    if (activeParticles.length + particlePool.length < MAX_PARTICLES) {
      let p = new Particle();
      p.pos.set(x, y);
      return p;
    } else {
      // si llegamos al límite, reciclar el más viejo (fallback)
      let oldestIdx = findOldestParticleIndex();
      if (oldestIdx >= 0) {
        let old = activeParticles[oldestIdx];
        old.reset();
        // quitar del array y devolver como "nuevo"
        activeParticles.splice(oldestIdx, 1);
        old.pos.set(x, y);
        return old;
      } else {
        // último recurso: crear pero podría pasar el limite; esto raramente ocurre
        return new Particle();
      }
    }
  }
}

function recycleParticle(index) {
  let p = activeParticles[index];
  if (p.system) p.system.particleCount = max(0, p.system.particleCount - 1);
  p.reset();
  particlePool.push(p);
  activeParticles.splice(index, 1);
}

function findOldestParticleIndex() {
  if (activeParticles.length === 0) return -1;
  let maxAge = -1, idx = 0;
  for (let i = 0; i < activeParticles.length; i++) {
    if (activeParticles[i].age > maxAge) {
      maxAge = activeParticles[i].age;
      idx = i;
    }
  }
  return idx;
}

function totalActiveParticles() {
  return activeParticles.length;
}

// -------------------- N-Body interactions (aprox) --------------------
// Aplicamos fuerzas gravitatorias pairwise entre partículas.
// Por performance, limitamos la cantidad de interacciones por frame (MAX_INTERACTIONS).
function applyNBodyForces() {
  let n = activeParticles.length;
  if (n < 2) return;

  // estrategia simple: si n grande, muestreamos pares aleatorios para aproximar
  if (n * (n - 1) / 2 <= MAX_INTERACTIONS) {
    // calcular todas las parejas
    for (let i = 0; i < n; i++) {
      let pi = activeParticles[i];
      for (let j = i + 1; j < n; j++) {
        let pj = activeParticles[j];
        applyGravityBetween(pi, pj);
      }
    }
  } else {
    // sampling aleatorio de parejas para aproximación
    for (let k = 0; k < MAX_INTERACTIONS; k++) {
      let i = int(random(n));
      let j = int(random(n));
      if (i === j) continue;
      applyGravityBetween(activeParticles[i], activeParticles[j]);
    }
  }
}

function applyGravityBetween(a, b) {
  // fuerza gravitacional suave: F = G * m1*m2 / (r^2 + eps)
  let dir = p5.Vector.sub(b.pos, a.pos);
  let dist2 = dir.magSq();
  let soft = SOFTENING * SOFTENING;
  let denom = dist2 + soft;
  if (denom <= 0) return;

  let forceMag = (G * a.mass * b.mass) / denom;
  // limitar fuerza para estabilidad
  if (forceMag > 500) forceMag = 500;
  dir.normalize();
  let f = dir.copy().mult(forceMag);

  // aplicar en sentidos opuestos
  a.applyForce(f);
  b.applyForce(f.mult(-1));
}

// -------------------- Utils --------------------
function colorFromHue(h) {
  // usa HSB para colores suaves
  colorMode(HSB, 360, 100, 100, 255);
  let c = color((h + 360) % 360, random(40, 80), random(50, 95));
  colorMode(RGB, 255);
  return c;
}

function keyPressed() {
  // tecla R limpia todo
  if (key === 'r' || key === 'R') {
    // reciclar activos
    while (activeParticles.length) {
      recycleParticle(activeParticles.length - 1);
    }
    systems = [];
  }
}
```  


  <img width="907" height="717" alt="image" src="https://github.com/user-attachments/assets/1b96b5b9-bc6f-4b71-88a1-c4471ee8c835" />


### Ejemplo 4.5: a Particle System with Inheritance and Polymorphism

- En este caso tenemos dor partículas diferentes, ``Particle`` y ``Confetti`` que estan en la misma clase ``this.particles.push``, lo que significa que existe un enparentamiento y polimorfismo.
- Las dos distintas partículas se matan de igual forma.
- Y Las dos partículas llegan a un mismo ``array`` y se elimina.

  ### Obra generativa 3:
  - Primero, las partículas nacen en el extremo del péndulo y van viviendo mientras caen y se van apagando poco a poco. Cuando su tiempo se acaba, las quito de la lista y desaparecen
  - En base a *Particle System with Inheritance and Polymorphism* quise simular una cascada de hielos y gotas de aguas en un pendulo, concepto de la unidad 4 asi cuando el pendulo se mueve la cascada también.
  - https://editor.p5js.org/ZombieYuu/sketches/GLoLgAnbG
  - Código:
``` js
// Péndulo invisible con generador de partículas en el extremo
// Las partículas caen en forma de cascada (agua + hielo)

let pendulum;
let system;

function setup() {
  createCanvas(900, 600);
  let origin = createVector(width / 2, 100); // origen del péndulo
  pendulum = new Pendulum(origin, 250); // un solo péndulo
  system = new ParticleSystem();
}

function draw() {
  background(20);

  // actualizar y mostrar péndulo
  pendulum.update();
  pendulum.display();

  // generar MUCHAS partículas cada frame (chorro fuerte)
  for (let i = 0; i < 20; i++) {   // 👈 ajusta este número para más/menos densidad
    system.addParticle(pendulum.getCenter());
  }

  // actualizar partículas
  system.run();

  // interacción con mouse
  if (pendulum.contains(mouseX, mouseY) && mouseIsPressed) {
    pendulum.drag(mouseX, mouseY);
  }
}

// -------------------- Pendulum --------------------
class Pendulum {
  constructor(origin, r) {
    this.origin = origin.copy();
    this.r = r;
    this.angle = PI / 4; // ángulo inicial
    this.aVel = 0;
    this.aAcc = 0;
    this.damping = 0.995; // fricción
  }

  update() {
    // física del péndulo
    let gravity = 0.4;
    this.aAcc = (-1 * gravity / this.r) * sin(this.angle);
    this.aVel += this.aAcc;
    this.aVel *= this.damping;
    this.angle += this.aVel;
  }

  display() {
    let pos = this.getCenter();
    stroke(200);
    strokeWeight(2);
    line(this.origin.x, this.origin.y, pos.x, pos.y);
    // no dibujamos el círculo (extremo invisible)
  }

  getCenter() {
    return createVector(
      this.origin.x + this.r * sin(this.angle),
      this.origin.y + this.r * cos(this.angle)
    );
  }

  contains(mx, my) {
    let d = dist(mx, my, this.getCenter().x, this.getCenter().y);
    return d < 20; // área "invisible" donde se puede agarrar
  }

  drag(mx, my) {
    // ángulo sigue al mouse
    let diff = p5.Vector.sub(this.origin, createVector(mx, my));
    this.angle = atan2(-diff.x, diff.y);
  }
}

// -------------------- Sistema de partículas --------------------
class ParticleSystem {
  constructor() {
    this.particles = [];
  }

  addParticle(pos) {
    // mitad hielo, mitad agua
    if (random(1) < 0.5) {
      this.particles.push(new Ice(pos));
    } else {
      this.particles.push(new Water(pos));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.display();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- Partícula base --------------------
class Particle {
  constructor(pos) {
    this.pos = pos.copy();
    // velocidad más ancha en horizontal (cascada dispersa)
    this.vel = createVector(random(-3, 3), random(1, 3));
    this.acc = createVector(0, 0.05); // gravedad
    this.lifespan = 255;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.lifespan -= 2;
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

// -------------------- Hielo (cuadrado blanco) --------------------
class Ice extends Particle {
  display() {
    noStroke();
    fill(255, this.lifespan);
    rectMode(CENTER);
    rect(this.pos.x, this.pos.y, 8, 8);
  }
}

// -------------------- Agua (círculo azul) --------------------
class Water extends Particle {
  display() {
    noStroke();
    fill(50, 150, 255, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 8, 8);
  }
}
```      
 
   <img width="863" height="595" alt="image" src="https://github.com/user-attachments/assets/0997af0c-5963-4871-ae0d-782937806d99" />


### Ejemplo 4.6:  a Particle System with Forces

- Las partículas se crean con ``add.Particle`` y pueden ser instancias de distintos tipos por las clases derivadas.
- Las particulas se matan de la misma manera.
- Se aplican fuerzas como la gravedad, pero no afecta en la gestión de la memoria, así mismo se maneja de la misma forma que los ejemplos anteriores.

  ### Obra generativa 4:
  - La estructura de gestion de partículas es similar a la que ya hemos visto antes, se crea una partícula, despues muere y se borra de la lista.
  - Con base en *a Particle System with Forces* utilicé el concepto de motion 101 de la unidad 2 y el random para tener diversos colores, con la letra C se borra la pantalla y con la R se agrega otra posición de emición.
  - https://editor.p5js.org/ZombieYuu/sketches/TvuMniRMM
  - Código:
``` js
// Generative artwork: "Motion101 + The Nature of Code (Example 4.6)"
// p5.js sketch (paste into https://editor.p5js.org/ or an HTML page with p5.js)
// Autor: ChatGPT — Código de ejemplo en español
// Objetivo: combinar el concepto de Motion 101 (principios básicos de movimiento)
// con el Example 4.6: A Particle System with Forces (The Nature of Code).

let emitter;
let gui = {};

function setup() {
  createCanvas(1080, 720);
  emitter = new Emitter(createVector(width / 2, 80));

  // Parámetros interactivos básicos
  gui.gravity = createSlider(-0.5, 2, 0.2, 0.01);
  gui.gravity.position(20, height - 90);
  gui.gravity.style('width', '200px');

  gui.wind = createSlider(-1, 1, 0.0, 0.01);
  gui.wind.position(20, height - 60);
  gui.wind.style('width', '200px');

  gui.rate = createSlider(0, 20, 6, 1);
  gui.rate.position(260, height - 90);
  gui.rate.style('width', '150px');

  gui.squash = createCheckbox('Squash & Stretch (size ~ speed)', true);
  gui.squash.position(260, height - 60);

  gui.windLabel = createDiv('Wind');
  gui.windLabel.position(20, height - 78);
  gui.gravityLabel = createDiv('Gravity');
  gui.gravityLabel.position(20, height - 108);
  gui.rateLabel = createDiv('Emission rate');
  gui.rateLabel.position(260, height - 108);

  // Un poco de estilo
  textFont('Arial');
}

function draw() {
  background(20, 24, 30, 60);

  // Forces
  let gravity = createVector(0, gui.gravity.value());
  let wind = createVector(gui.wind.value(), 0);

  // Aplicar fuerzas al emisor (que a su vez aplica a todas las particulas)
  emitter.applyForce(gravity);
  emitter.applyForce(wind);

  // Emisión de partículas según el slider
  for (let i = 0; i < gui.rate.value(); i++) {
    emitter.addParticle();
  }

  emitter.run();

  // UI simple
  noStroke();
  fill(255);
  textSize(12);
  text('Partículas vivas: ' + emitter.particles.length, 20, 20);
  text('Presiona C para limpiar, R para reiniciar posición del emisor\n(usa ratón para mover el emisor arrastrando).', 20, 36);
}

// Mover emisor con mouse
function mouseDragged() {
  if (mouseY < height - 120) {
    emitter.origin.set(mouseX, mouseY);
  }
}

function keyPressed() {
  if (key === 'C' || key === 'c') {
    emitter.clear();
  }
  if (key === 'R' || key === 'r') {
    emitter.origin.set(width / 2, 80);
  }
}

// ------------------ CLASES ------------------

class Particle {
  constructor(position) {
    this.position = position.copy();
    // velocity inicial aleatoria para simular "anticipation" (pequeña antes del estallido)
    this.velocity = p5.Vector.random2D().mult(random(0.5, 2));
    // masa influye en cómo responden a las fuerzas (Motion: "weight")
    this.mass = random(0.6, 2.0);
    this.acceleration = createVector(0, 0);
    this.lifespan = 255; // decrece
    // tamaño base
    this.baseSize = random(4, 18);
    // color (tonos cálidos para movimiento, puedes cambiar)
    this.h = random(0, 360);
    this.s = random(60, 100);
    this.b = random(60, 100);

    // para follow-through / trailing: history
    this.history = [];
  }

  applyForce(force) {
    // F = ma -> a = F / m
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  run() {
    this.update();
    this.display();
  }

  update() {
    // Integración física básica
    this.velocity.add(this.acceleration);

    // --- Motion 101: easing / drag (follow-through)
    // aplicamos un arrastre (damping) para simular fricción y que haya follow-through
    let damping = 0.985; // entre 0.95 y 0.995
    this.velocity.mult(damping);

    this.position.add(this.velocity);
    this.acceleration.mult(0);

    // lifespan decrementa más rápido si la velocidad es mayor (evita partículas infinitas)
    this.lifespan -= map(this.velocity.mag(), 0, 10, 0.4, 3, true);

    // guardar history para estelas (secondary motion)
    this.history.push(this.position.copy());
    if (this.history.length > 12) this.history.shift();
  }

  display() {
    // Mapear velocidad a tamaño para squash & stretch
    let speed = this.velocity.mag();
    let sizeX = this.baseSize;
    let sizeY = this.baseSize;

    if (gui.squash.checked()) {
      // Partícula "estirada" en dirección del movimiento
      sizeY = this.baseSize + speed * 4; // stretch
      sizeX = this.baseSize - speed * 0.6; // squash
      sizeX = max(1, sizeX);
    }

    // Orientación: alinear el elipse con la dirección del movimiento
    let angle = this.velocity.heading();

    push();
    translate(this.position.x, this.position.y);
    rotate(angle);

    // Draw trail (secondary motion, follow-through)
    noStroke();
    for (let i = 0; i < this.history.length; i++) {
      let pos = this.history[i];
      let alpha = map(i, 0, this.history.length - 1, 10, this.lifespan);
      let sz = map(i, 0, this.history.length - 1, sizeX * 0.4, sizeX);
      push();
      translate(pos.x - this.position.x, pos.y - this.position.y);
      colorMode(HSB, 360, 100, 100, 255);
      fill(this.h, this.s, this.b, alpha * 0.6);
      ellipse(0, 0, sz, sz * 0.5);
      pop();
    }

    // Partícula principal
    colorMode(HSB, 360, 100, 100, 255);
    fill(this.h, this.s, this.b, this.lifespan);
    noStroke();
    ellipse(0, 0, sizeX, sizeY);

    pop();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

class Emitter {
  constructor(position) {
    this.origin = position.copy();
    this.particles = [];
    // small random spread for anticipation
    this.spread = PI / 6;
  }

  applyForce(force) {
    // Aplica la fuerza a cada partícula (seguimiento del Example 4.6)
    for (let p of this.particles) {
      p.applyForce(force);
    }
  }

  addParticle() {
    // Emitir con pequeña variación para dar sensación de "anticipation" y "arc"
    let p = new Particle(this.origin);
    // podemos dar un empujón inicial (burst) aleatorio
    let burst = p5.Vector.random2D().mult(random(0.5, 3));
    // bias hacia abajo para simular gravedad en general
    burst.y = abs(burst.y) * random(0.2, 1.5);
    p.applyForce(burst);

    this.particles.push(p);
  }

  run() {
    // actualizar y mostrar todas las partículas; eliminar las muertas
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

  clear() {
    this.particles = [];
  }
}

```  
 
   <img width="898" height="755" alt="image" src="https://github.com/user-attachments/assets/a1c800aa-56f0-4800-9e9c-291400a5e664" />


### Ejemplo 4.7: a Particle System with a Repeller

- La creación de partículas funciona de la misma forma que en los ejemplos anteriores.
- La muerte de las partículas es igual.
- Se agrega una nueva fuerza de repulción, no afecta a la gestión de memoria pero las partículas muertas se eliminan por medio de ``Particles.remove(i)``.

  ### Obra generativa 5:
  - La gestión de partículas es igual que la anterior obra generativa, pero aquí la cantidad de partículas limita hasta 1020.
  - En base a *a Particle System with a Repeller* utilicé conceptos como el ruido de perlin y la atracción gravitacional para crear la obra.
  - https://editor.p5js.org/ZombieYuu/sketches/MfuVgbPxf
  - Código:
```   js
  // Generative artwork: "Perlin Noise + Particle System with Repeller (Example 4.7)"
// p5.js sketch — combina ruido de Perlin en el movimiento de partículas
// con el concepto de un Repeller más grande que empuja las partículas.

let emitter;
let repeller;

function setup() {
  createCanvas(1080, 720);
  emitter = new Emitter(createVector(width / 2, 50));
  repeller = new Repeller(width / 2, height / 2, 80); // repeller grande
}

function draw() {
  background(20, 24, 30, 60);

  // Emite nuevas partículas
  for (let i = 0; i < 4; i++) { // menos partículas para que se aprecie mejor
    emitter.addParticle();
  }

  // Fuerza de repulsión
  let force = repeller.repel(emitter.particles);

  // Aplica la fuerza del repeller a cada partícula
  for (let p of emitter.particles) {
    p.applyForce(force(p));
  }

  emitter.run();
  repeller.display();

  fill(255);
  noStroke();
  textSize(12);
  text('Partículas vivas: ' + emitter.particles.length, 20, 20);
}

// ------------------ CLASES ------------------

class Particle {
  constructor(position) {
    this.position = position.copy();
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    this.mass = 1;

    // valores iniciales para ruido de Perlin
    this.xoff = random(1000);
    this.yoff = random(1000);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  run() {
    this.update();
    this.display();
  }

  update() {
    // Movimiento influenciado por ruido de Perlin más suave
    let angle = noise(this.xoff, this.yoff) * TWO_PI * 2;
    let perlinVel = p5.Vector.fromAngle(angle);
    perlinVel.mult(0.2); // velocidad reducida

    this.velocity.add(this.acceleration);
    this.velocity.add(perlinVel);
    this.velocity.limit(2); // límite de velocidad
    this.position.add(this.velocity);
    this.acceleration.mult(0);

    this.lifespan -= 1.0; // vida más larga

    this.xoff += 0.01;
    this.yoff += 0.01;
  }

  display() {
    stroke(255, this.lifespan);
    fill(127, this.lifespan);
    ellipse(this.position.x, this.position.y, 8, 8);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

class Emitter {
  constructor(position) {
    this.origin = position.copy();
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

class Repeller {
  constructor(x, y, r) {
    this.position = createVector(x, y);
    this.r = r; // radio grande
    this.strength = 150; // un poco menos de fuerza
  }

  repel(particles) {
    return (p) => {
      let dir = p5.Vector.sub(p.position, this.position);
      let d = dir.mag();
      d = constrain(d, 10, 150);
      dir.normalize();
      let force = -1 * this.strength / (d * d);
      dir.mult(force);
      return dir;
    };
  }

  display() {
    noStroke();
    fill(200, 50, 50, 180);
    ellipse(this.position.x, this.position.y, this.r * 2, this.r * 2);
  }
}

```  
 
   <img width="917" height="770" alt="image" src="https://github.com/user-attachments/assets/2e0491c3-2571-44ba-9b54-a9668350c3b8" />

## Actividad 3: Apply

 ### Conceprto y Boceto: 

Mi concepto para esta obra es de varias medusas y burbujas nadando en el mar que nadan a la velocidad de la música, con la tecla *c* se cambia el color de las medusas y con la tecla epaciadora se pausa la música

<img width="2975" height="3850" alt="Boceto obra" src="https://github.com/user-attachments/assets/0414b679-784b-4b88-9c02-c37ba6ee90fa" />

## Conceptos usados: 

- **Unidad 1:** random para cambio de color
- **Unidad 2:** vectores para movimiento.
- **Unidad 3:** fuerzas de flotabilidad y corriente.
- **Unidad 4:** oscilaciones para tentáculos y pulso.
- **Unidad 5:** sistemas de partículas con vida útil y limpieza.
- **Herencia y polimorfismo:** Particle → JellyParticle y BubbleParticle con comportamientos distintos.
- **Gestión de vida y memoria:** reducción de lifespan y eliminación de partículas muertas.


## Código: 
``` js
// 🪼 Medusas en la Corriente - Generative Art con p5.js
// Requiere p5.sound

let song, fft, amp;
let systems = [];
let fileInput;
let playing = false;
let jellyHue = 200; // color inicial de medusas

function setup() {
  createCanvas(900, 600);
  colorMode(HSB, 360, 100, 100, 100);
  background(220, 50, 20);

  fileInput = createFileInput(handleFile);
  fileInput.position(10, 10);

  fft = new p5.FFT(0.8, 64);
  amp = new p5.Amplitude();
}

function draw() {
  background(220, 50, 20, 15); // Persistencia estilo acuático

  if (playing) {
    let level = amp.getLevel();

    // Crear sistemas de partículas (más nivel -> más medusas y burbujas)
    if (frameCount % int(map(level, 0, 0.5, 20, 5)) === 0) {
      let origin = createVector(random(width), height + 20);
      systems.push(new ParticleSystem(origin, level));
    }

    // Actualizar y dibujar sistemas
    for (let i = systems.length - 1; i >= 0; i--) {
      systems[i].run(level);
      if (systems[i].isDead()) {
        systems.splice(i, 1);
      }
    }
  }
}

// ----------------------
// Clases de Partículas
// ----------------------
class Particle {
  constructor(pos) {
    this.pos = pos.copy();
    this.vel = createVector(random(-0.5, 0.5), random(-1.5, -0.5));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 1.2;
  }

  isDead() {
    return this.lifespan < 0;
  }

  display() {
    // Se sobrescribe en subclases
  }
}

// 🪼 Medusa
class JellyParticle extends Particle {
  constructor(pos) {
    super(pos);
    this.size = random(20, 40);
    this.offset = random(TWO_PI);
    this.tentacles = int(random(6, 12));
  }

  display(level) {
    push();
    translate(this.pos.x, this.pos.y);

    // Pulso controlado por audio
    let pulse = map(level, 0, 0.5, 0.8, 1.5);
    let r = this.size * pulse;

    // Cuerpo de la medusa
    noStroke();
    fill(jellyHue, 80, 100, this.lifespan * 0.4);
    ellipse(0, 0, r * 1.2, r);

    // Tentáculos ondulantes (más largos ahora)
    stroke(jellyHue, 60, 90, this.lifespan * 0.5);
    strokeWeight(1.5);
    for (let i = 0; i < this.tentacles; i++) {
      let angle = map(i, 0, this.tentacles, -PI/2, PI/2);
      beginShape();
      for (let y = 0; y < r * 4; y += 5) { // antes r*2, ahora r*4 para más largos
        let x = sin(this.offset + frameCount * 0.05 + y * 0.1) * 10;
        vertex(cos(angle) * x, y);
      }
      endShape();
    }

    pop();
  }
}

// 🫧 Burbuja
class BubbleParticle extends Particle {
  constructor(pos) {
    super(pos);
    this.size = random(5, 12);
  }

  display(level) {
    noFill();
    stroke(195, 30, 100, this.lifespan * 0.5);
    ellipse(this.pos.x, this.pos.y, this.size + level * 50);
  }
}

// Sistema de partículas
class ParticleSystem {
  constructor(origin, level) {
    this.origin = origin.copy();
    this.particles = [];
    for (let i = 0; i < int(random(1, 3)); i++) {
      this.addParticle(level);
    }
  }

  addParticle(level) {
    if (random(1) < 0.6) {
      this.particles.push(new JellyParticle(this.origin));
    } else {
      this.particles.push(new BubbleParticle(this.origin));
    }
  }

  run(level) {
    let buoyancy = createVector(0, -0.02); // Flotabilidad
    let drag = createVector(random(-0.01, 0.01), 0); // Corriente lateral

    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.applyForce(buoyancy);
      p.applyForce(drag);
      p.update();
      p.display(level);
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

  isDead() {
    return this.particles.length === 0;
  }
}

// ----------------------
// Funciones de Audio
// ----------------------
function handleFile(file) {
  if (file.type === 'audio') {
    if (song) song.stop();
    song = loadSound(file.data, () => {
      song.loop();
      playing = true;
    });
  }
}

// ----------------------
// Controles con teclado
// ----------------------
function keyPressed() {
  if (key === 'c' || key === 'C') {
    jellyHue = random(0, 360); // Cambiar color de medusas
  }

  if (key === ' ') { // barra espaciadora
    if (song && song.isPlaying()) {
      song.pause();
    } else if (song) {
      song.play();
    }
  }
}
```

## Enlace: 

https://editor.p5js.org/ZombieYuu/sketches/57nnAFtB7

## Imagen: 

<img width="935" height="745" alt="image" src="https://github.com/user-attachments/assets/5fb4bf05-a595-49f3-9f90-fae423320dd7" />


## Evaluación Autónoma: 

1. **Investigación y Experimentación (Evidencia en Actividad 2):** 5.0, porque hice la investigación asignada entendiendo el contenido que esbamos estudiando, también hice experimentaciones que me aydaron a entender mejor el concepto.
2. **Intención y Diseño (Proceso de Actividad 3):** 5.0, porque traté de pasmar una idea en un concepto mas sencillo en p.5 js, diseñando la visual de la obra por medio de un boceto.
4. **3. Aplicación Técnica (Código de Actividad 3):** 5.0, Al incluir diversos conceptos de unidades anteriores pude recrear mi idea de una manera mas abstracta por medio del codigo en p.5js.
5. **Calidad de la Obra Final (Artefacto Entregado)** 5.0, El resultado de la obra es satisfactorio pues cumple con todos los criterios de la actividad.

**Nota final** 5.0.







