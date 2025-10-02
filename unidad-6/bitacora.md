# Evidencias de la unidad 6

### Actividad 1:

<img width="747" height="744" alt="image" src="https://github.com/user-attachments/assets/4603df79-f5ec-4ae9-ae2b-5cb492036a1a" />
<img width="748" height="559" alt="image" src="https://github.com/user-attachments/assets/d7c0e773-ea32-42c6-b1e7-22712ce82135" />

Del trabajo de Tyler me llama mucho la atención la manera tan orgánica en la que se ve, los elementos parecen tener vida. Me inspira paz y belleza, y me gustaría aplicarlo en mis obras.


## Seet and Seek:

### Actividad 2:

- **¿Qué es una fuerza de dirección (steering force)?**
  Es una fuerza vectorial que modifica la dirección de las partículas teneiendo en cuenta la fórmula de ``desired - velocity``. 
- **¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?**
  A diferencia de las fuerzas que hemos trabajado anteriormente, esta es una fuerza cambiante e interna del vehículo mientras que las anteriores como la gravedad eran fuerzas externas.
- **¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?**
  Se relacionan porque usan el mismo modelo para simular el comportamiento animal en grupo, con 3 simples reglas: separación (evitar chocar con otros), alineación (moverse en la misma dirección), y cohesión (mantenerse en grupo).

### Actividad 3:

- **Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores**
  El campo de flujo se almacena en un ``array`` que contiene un vector, este ``array`` en realidad es una matriz 2D que en cada celda hay un vector con una dirección, los vectores se crean en este caso simulando el ruido de perlin con un ``noise`` y se crea en el método ``init`` para crear una variación suave. Luego se transforma en un ángulo.
- **Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección**
  El agente usa su posición actual para movilizarse y autiliza el vector de abajo para direccionar su movimiento y así seguir el campo de flujo. Se cacula restando la velocidad actual a la velocidad deseada, se limita por el ``maxForce``
- **Lista los parámetros clave identificados (resolución, maxspeed, maxforce)**
  La resolución nos epecifica el tamaño del campo de flujo por ejemplo 200x200, la maxspeed es la máxima velocidad que puede tener el vehículo y maxforce es la fuerza máxima de dirección.
- **Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio** - **Muestra el fragmento de código modificado**

**Efecto de la modificación**

**Aumento en la cantidad de agentes (200 en lugar de 120):**

- El lienzo se llena mucho más de partículas.

- El patrón del flow field se hace más visible porque los agentes resaltan las líneas del campo vectorial con mayor densidad.

**Aumento en la velocidad máxima (maxspeed de 4–8 en lugar de 2–5):

- Los vehículos se desplazan más rápido.

- Las trayectorias parecen más caóticas y dinámicas.

**Aumento en la fuerza máxima (maxforce de 0.5–1 en lugar de 0.1–0.5):**

- Los agentes cambian de dirección de forma más brusca al seguir los vectores del campo.

- Esto rompe un poco la suavidad del movimiento y crea una sensación de enjambre inquieto.

**En resumen:**

La modificación provoca que los agentes se vean más numerosos, más veloces y con giros más agresivos, lo que cambia la percepción de un movimiento “fluido y suave” a un comportamiento más caótico y energético, parecido a un enjambre de insectos o un banco de peces en estado de agitación.

**Modificación Realizada:**

```js
// ANTES
for (let i = 0; i < 120; i++) {
  vehicles.push(
    new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5))
  );
}

// DESPUÉS
for (let i = 0; i < 200; i++) {
  vehicles.push(
    new Vehicle(random(width), random(height), random(4, 8), random(0.5, 1))
  );
}

```

**Captura**

<img width="806" height="294" alt="image" src="https://github.com/user-attachments/assets/edbcfdb8-2b08-4c52-a9d0-55072a02c5e9" />
https://editor.p5js.org/ZombieYuu/sketches/kaeGdLGyi

### Actividad 4: 

- **Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión):**

  **1. Separación:**

Objetivo: Evitar que los boids choquen o se amontonen demasiado.

Lógica:
Cada boid mira a su alrededor dentro de un radio definido.
Si otro boid está demasiado cerca, calcula un vector que lo aleje (repulsión).
Mientras más cerca esté el vecino, más fuerte es el empuje.
→ Resultado: los boids mantienen distancia personal, evitando colisiones.

**2. Alineación:**

Objetivo: Que cada boid intente volar en la misma dirección que sus vecinos.

Lógica:
El boid revisa el promedio de las velocidades de los boids cercanos dentro de su radio.
Luego ajusta su dirección para alinearse con ese vector promedio.
→ Resultado: los boids tienden a moverse en paralelo, como aves en formación.

**3. Cohesión:**

Objetivo: Mantener a la bandada unida, evitando que los boids se dispersen demasiado.

Lógica:
El boid calcula el centro de masa de sus vecinos en el radio de percepción.
Luego genera un vector que lo dirige hacia ese centro (atracción).
→ Resultado: los boids se agrupan y mantienen la bandada cohesionada.

  
- **Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce):**

**1. Radio de percepción:**

Define hasta qué distancia un boid considera a otros como “vecinos”.

Puede ser distinto para cada regla (ej. mayor para cohesión, menor para separación).

**2. Pesos de las reglas:**

- Cada fuerza (separación, alineación, cohesión) se multiplica por un factor de peso.

- Controla la importancia relativa: por ejemplo, darle más peso a separación hace que eviten choques más agresivamente.

**3. maxspeed:**

- Limita la rapidez con que un boid puede moverse.

- Evita que se acelere demasiado y da estabilidad al sistema.

**4. maxforce:**

- Limita cuánto puede cambiar de dirección el boid en cada frame.

- Si es bajo → movimientos suaves; si es alto → cambios bruscos de rumbo.

- **Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el comportamiento colectivo del enjambre (¿Se dispersan? ¿Forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado**

**Explicación del efecto**

**Separación más fuerte (2.5):**
Los boids se repelen más cuando están cerca → ya no forman un enjambre tan apretado.

**Cohesión más débil (0.5):**
Disminuye la fuerza que los atrae hacia el centro del grupo → se dispersan más en el espacio.

**Alineación igual (1.0):**
Mantienen direcciones similares, así que aún se percibe cierto “vuelo en formación”, pero con mucho más espacio entre ellos.

Todos estos factores hacen que los cuerpos se dispersen y que no formen grupos compactos.

**Código**

```
// ANTES
sep.mult(1.5);
ali.mult(1.0);
coh.mult(1.0);

// DESPUÉS (más separación, menos cohesión)
sep.mult(2.5);
ali.mult(1.0);
coh.mult(0.5);
```

**Captura**

<img width="795" height="301" alt="image" src="https://github.com/user-attachments/assets/6f2e8951-4f42-4474-b769-47e4d64d0025" />
https://editor.p5js.org/ZombieYuu/sketches/bju-x_7Uo

### Actividad 5: Apply

Mi concepto combina el flow fields y el flocking en una misma obra, donde las estrellas son las protagonistas y palpitan al ritmo de la música.

**Boceto**
<img width="1920" height="1080" alt="Aply 6" src="https://github.com/user-attachments/assets/365dfab1-c488-429d-8573-095a51c9ac0f" />

**Codigo**

```
// 🎵 Generative Stars with Flow Field + Music Interaction
// Usa p5.js + p5.sound

let vehicles = [];
let flowfield;
let fft;
let bassEnergy = 0;
let useColor = true;
let sound;  // para la música

const NUM_BOIDS = 40;   // menos estrellas, más separadas
const FLOW_RES = 30;

function setup() {
  createCanvas(960, 540);
  pixelDensity(1);
  colorMode(HSB, 360, 100, 100, 255);

  flowfield = new FlowField(FLOW_RES);
  for (let i = 0; i < NUM_BOIDS; i++) {
    vehicles.push(new Vehicle(random(width), random(height)));
  }

  // FFT para analizar música
  fft = new p5.FFT(0.8, 1024);

  // Botón para cargar música
  let fileInput = createFileInput(handleFile);
  fileInput.position(10, 10);

  background(0);
}

function draw() {
  // Fondo semitransparente → estela
  noStroke();
  fill(0, 0, 0, 40);
  rect(0, 0, width, height);

  // Analizar música
  if (sound && sound.isPlaying()) {
    let spectrum = fft.analyze();
    bassEnergy = fft.getEnergy(20, 150); // graves 20-150Hz
  } else {
    bassEnergy = 0;
  }

  let bassFactor = map(bassEnergy, 0, 255, 0.8, 3);

  // Actualizar vehículos (estrellas)
  for (let v of vehicles) {
    v.follow(flowfield);
    v.flock(vehicles);
    v.update(bassFactor);
    v.borders();
    v.render();
  }
}

// 🔹 Cambiar camino del flowfield con click
function mousePressed() {
  flowfield.init(random(10000));
}

// 🔹 Activar/desactivar modo colorido con "C"
function keyPressed() {
  if (key === "c" || key === "C") {
    useColor = !useColor;
  }
}

// 🔹 Manejar archivo de audio cargado
function handleFile(file) {
  if (file.type === 'audio') {
    if (sound) {
      sound.stop();
    }
    sound = loadSound(file.data, () => {
      sound.loop();
    });
  }
}

// ----------------------------------------------------
// FlowField
// ----------------------------------------------------
class FlowField {
  constructor(res) {
    this.res = res;
    this.cols = floor(width / this.res);
    this.rows = floor(height / this.res);
    this.field = this.make2DArray(this.cols);
    this.init(random(10000));
  }

  make2DArray(n) {
    let arr = [];
    for (let i = 0; i < n; i++) {
      arr[i] = [];
    }
    return arr;
  }

  init(zstart = 0) {
    let xoff = 0;
    noiseDetail(4, 0.5);
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        let angle = noise(xoff, yoff, zstart) * TWO_PI * 4;
        let v = p5.Vector.fromAngle(angle);
        this.field[i][j] = v;
        yoff += 0.1;
      }
      xoff += 0.1;
    }
  }

  lookup(lookup) {
    let column = int(constrain(lookup.x / this.res, 0, this.cols - 1));
    let row = int(constrain(lookup.y / this.res, 0, this.rows - 1));
    return this.field[column][row].copy();
  }
}

// ----------------------------------------------------
// Vehicle (estrella)
// ----------------------------------------------------
class Vehicle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.maxspeed = 2;
    this.maxforce = 0.1;
    this.r = 8; // radio base más grande
  }

  applyForce(force) {
    this.acc.add(force);
  }

  follow(flow) {
    let desired = flow.lookup(this.pos);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.vel);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  flock(vehicles) {
    let sep = this.separate(vehicles);
    let ali = this.align(vehicles);
    let coh = this.cohesion(vehicles);

    // pesos
    sep.mult(1.8);   // separación más fuerte
    ali.mult(0.8);
    coh.mult(0.9);

    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
  }

  update(bassFactor) {
    this.vel.add(this.acc);
    this.vel.limit(this.maxspeed * bassFactor); // más velocidad con bajos
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  borders() {
    if (this.pos.x < -this.r) this.pos.x = width + this.r;
    if (this.pos.y < -this.r) this.pos.y = height + this.r;
    if (this.pos.x > width + this.r) this.pos.x = -this.r;
    if (this.pos.y > height + this.r) this.pos.y = -this.r;
  }

  render() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.vel.heading());

    let size = this.r + map(bassEnergy, 0, 255, 0, 12); // palpitar con graves

    if (useColor) {
      // ciclo de color dinámico arcoíris
      let hue = (frameCount + this.pos.x * 0.2 + this.pos.y * 0.2) % 360;
      fill(hue, 80, 100, 220);
      stroke(0, 0, 100, 180);
    } else {
      fill(0, 0, 100, 200); // blanco
      stroke(0, 0, 100);
    }

    this.drawStar(0, 0, size * 0.5, size, 5);
    pop();
  }

  // ⭐ función para dibujar estrellas
  drawStar(x, y, radius1, radius2, npoints) {
    let angle = TWO_PI / npoints;
    let halfAngle = angle / 2.0;
    beginShape();
    for (let a = 0; a < TWO_PI; a += angle) {
      let sx = x + cos(a) * radius2;
      let sy = y + sin(a) * radius2;
      vertex(sx, sy);
      sx = x + cos(a + halfAngle) * radius1;
      sy = y + sin(a + halfAngle) * radius1;
      vertex(sx, sy);
    }
    endShape(CLOSE);
  }

  // ---- reglas de flocking ----
  separate(vehicles) {
    let desiredSeparation = this.r * 4; // más distancia
    let sum = createVector();
    let count = 0;
    for (let other of vehicles) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < desiredSeparation) {
        let diff = p5.Vector.sub(this.pos, other.pos);
        diff.normalize();
        diff.div(d);
        sum.add(diff);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.setMag(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.vel);
      steer.limit(this.maxforce);
      return steer;
    }
    return createVector(0, 0);
  }

  align(vehicles) {
    let neighbordist = 60;
    let sum = createVector();
    let count = 0;
    for (let other of vehicles) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < neighbordist) {
        sum.add(other.vel);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.setMag(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.vel);
      steer.limit(this.maxforce);
      return steer;
    }
    return createVector(0, 0);
  }

  cohesion(vehicles) {
    let neighbordist = 60;
    let sum = createVector();
    let count = 0;
    for (let other of vehicles) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < neighbordist) {
        sum.add(other.pos);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum);
    }
    return createVector(0, 0);
  }

  seek(target) {
    let desired = p5.Vector.sub(target, this.pos);
    desired.setMag(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.vel);
    steer.limit(this.maxforce);
    return steer;
  }
}
```

**Imagen**
<img width="861" height="668" alt="image" src="https://github.com/user-attachments/assets/a3805209-3d55-4630-84c2-ed7c0af95f6d" />
https://editor.p5js.org/ZombieYuu/sketches/ZrJHCNBjf

### Autoevaluación:

5.0, pues realicé todas las actividades con los requerimientos.



