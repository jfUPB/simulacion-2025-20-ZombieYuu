
# Evidencias de la unidad 8

## Actividad 1 Seet:

- **De Sónar+D CCCB 2020:** me parece muy interesante el movimiento y como salen los pequeños gusanos al ritmo de las vibraciónes y como todo se complementa para ser armónico y tranquilo, los elementos generativos son las lineas que aparecen en el suelo y rodean al músico, siempre tendrán una forma diferente, lo mismo con los gusanos que vibran, aparecerán en un lugar diferente cada vez que se toque la obra, mi reflexión es que el arte generativo puede ser único y relajante, a su vez novedoso domde hay una gran conexión con música, arte y alma.
- **De Dimension N:** me gusta la forma con la que juegan con la simetría y el tamaño de las figuras al ritmo y el pasar de la m{usica, pues cada vez que se toque esta obra sera con formas, colores y tamaños diferentes, su relación con el sonido lo hace entretenido y acorde, dando a su vez tranquilidad.

  ## Actividad 2 Seek:

La pieza musical que utilizaré para mi obra es: https://youtu.be/oR_oQIK5v5A?si=et6yMjdFmCO_tpuK, y quiero hacer que unas arañas vayan saliendo en la pantalla tejiendo una telaraña,los inputs que utilizaré seran la música para que la telaraña vibre con los bajos, la tecla *c* del teclado para cambiar el color a mi gusto y unos sliders que usaré para modificar parámetros como la cantidad de arañas y velocidad de las arañas.

### Los algoritmos que utilizaré son:

| Concepto del libro                     | Aplicación en tu obra                                |
| -------------------------------------- | ---------------------------------------------------- |
| **Vectores**                           | Posición, velocidad y dirección de las arañas        |
| **Movimiento y fuerzas**               | Movimiento natural con rebotes y aceleración         |
| **Ruido Perlin / oscilación**          | Vibración orgánica de hilos y deformación de la red  |
| **Sistemas de partículas**             | Estelas de las arañas                                |
| **Tiempo independiente de frame rate** | Movimiento fluido y estable                          |
| **Interacción y entrada externa**      | Reacción en tiempo real a la música                  |
| **Comportamiento emergente**           | Red viva generada por múltiples arañas en movimiento |

### Boceto:
<img width="1920" height="1080" alt="Telaraña" src="https://github.com/user-attachments/assets/34fae7a0-98b5-4ed3-89af-b2b3052b3f21" />


## Actividad 3: Apply 

### Código: 

**En ```index.htlm```**

``` javaScript
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Telarañas Sonoras</title>
  <style>
    body {
      margin: 0;
      background: #000;
      color: #fff;
      font-family: sans-serif;
      overflow: hidden;
    }

    canvas {
      display: block;
    }

    /* Panel lateral izquierdo */
    #ui {
      position: fixed;
      top: 0;
      left: 0;
      width: 260px;
      height: 100vh;
      background: rgba(0, 0, 0, 0.5);
      backdrop-filter: blur(8px);
      padding: 16px;
      box-sizing: border-box;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: flex-start;
      z-index: 10;
      border-right: 1px solid rgba(255, 255, 255, 0.1);
    }

    label {
      display: block;
      margin-bottom: 10px;
      width: 100%;
      text-align: left;
      font-size: 0.9em;
    }

    input[type="range"] {
      width: 100%;
      margin-top: 4px;
    }

    #fileinput, #playBtn {
      margin-bottom: 12px;
      width: 100%;
    }

    #playBtn {
      background: rgba(255, 255, 255, 0.15);
      border: none;
      color: #fff;
      padding: 8px;
      border-radius: 6px;
      cursor: pointer;
      transition: background 0.2s;
    }

    #playBtn:hover {
      background: rgba(255, 255, 255, 0.25);
    }

    .sliderVal {
      color: #ffcc66;
      font-weight: bold;
      float: right;
    }

    .small {
      font-size: 0.8em;
      color: #aaa;
      margin-top: 4px;
    }

    #credits {
      position: absolute;
      bottom: 10px;
      left: 16px;
      font-size: 0.8em;
      color: #888;
    }
  </style>
</head>
<body>
  <div id="ui">
    <label>
      <input id="fileinput" type="file" accept="audio/*">
    </label>

    <button id="playBtn">▶️ Play / Pausar</button>

    <label> # Arañas <span id="spiderCountVal" class="sliderVal"></span>
      <input id="spiderCount" type="range" min="1" max="60" value="12">
    </label>

    <label> Sensibilidad latido <span id="sensitivityVal" class="sliderVal"></span>
      <input id="sensitivity" type="range" min="0" max="3" step="0.01" value="1.0">
    </label>

    <label> Velocidad arañas <span id="speedVal" class="sliderVal"></span>
      <input id="speedSlider" type="range" min="0.2" max="5" step="0.1" value="1">
    </label>

    <label class="small">Presiona “c” para cambiar color de telaraña</label>
    <label class="small">Presiona “r” para limpiar estelas</label>
    <label class="small">Click sobre el lienzo si no suena el audio</label>

    <div id="credits">Arte generativo • p5.js + p5.sound</div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/p5@1.6.0/lib/p5.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/p5@1.6.0/lib/addons/p5.sound.min.js"></script>
  <script src="sketch.js"></script>
</body>
</html>
```

**En ```Sketch.js```**

```javaScript
// sketch.js — Versión con slider de velocidad + estelas traslúcidas
let audio, fileInput, playBtn;
let fft, amp;
let spiders = [];
let trailSegments = [];

let baseWeb = { spokes: 12, rings: 5, radius: 220, nodes: [] };
let center;

let spiderCountSlider, sensitivitySlider, speedSlider;
let spiderCountVal, sensitivityVal, speedVal;

let spiderSpeed = 1.0;

let colorIndex = 0;
let webColors = [
  [255, 255, 255, 200],
  [180, 220, 255, 230],
  [255, 200, 230, 230],
  [200, 255, 200, 230],
  [255, 230, 150, 230]
];
let currentColor = webColors[0].slice();

let beatSmooth = 0;
let prevEnergy = 0;
let maxBeat = 6.0;

function setup() {
  createCanvas(windowWidth, windowHeight);
  pixelDensity(1);
  center = createVector(width / 2, height / 2);

  fileInput = select('#fileinput');
  playBtn = select('#playBtn');
  spiderCountSlider = select('#spiderCount');
  sensitivitySlider = select('#sensitivity');
  speedSlider = select('#speedSlider');

  spiderCountVal = select('#spiderCountVal');
  sensitivityVal = select('#sensitivityVal');
  speedVal = select('#speedVal');

  fileInput.changed(handleFile);
  playBtn.mousePressed(togglePlay);

  fft = new p5.FFT(0.8, 512);
  amp = new p5.Amplitude(0.8);

  buildBaseWeb();

  spiderCountSlider.input(() => {
    updateSpidersCount(parseInt(spiderCountSlider.value()));
    spiderCountVal.html(spiderCountSlider.value());
  });

  sensitivitySlider.input(() => {
    sensitivityVal.html(sensitivitySlider.value());
  });

  speedSlider.input(() => {
    spiderSpeed = parseFloat(speedSlider.value());
    speedVal.html(speedSlider.value());
  });

  updateSpidersCount(parseInt(spiderCountSlider.value()));
  spiderCountVal.html(spiderCountSlider.value());
  sensitivityVal.html(sensitivitySlider.value());
  speedVal.html(speedSlider.value());

  userStartAudio();
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  center.set(width / 2, height / 2);
  buildBaseWeb();
}

function buildBaseWeb() {
  baseWeb.nodes = [];
  let spokes = baseWeb.spokes;
  let rings = baseWeb.rings;
  baseWeb.radius = min(width, height) * 0.36;
  let rStep = baseWeb.radius / rings;
  for (let i = 0; i <= rings; i++) {
    let r = rStep * i;
    let ring = [];
    for (let s = 0; s < spokes; s++) {
      let theta = map(s, 0, spokes, 0, TWO_PI);
      let x = center.x + r * cos(theta);
      let y = center.y + r * sin(theta);
      ring.push(createVector(x, y));
    }
    baseWeb.nodes.push(ring);
  }
}

function updateSpidersCount(n) {
  if (spiders.length < n) {
    for (let i = spiders.length; i < n; i++) spiders.push(new Spider());
  } else {
    spiders.splice(n);
  }
}

function handleFile() {
  let f = fileInput.elt.files[0];
  if (!f) return;
  if (audio && audio.isPlaying()) audio.stop();
  if (audio) audio.disconnect();

  let url = URL.createObjectURL(f);
  audio = loadSound(
    url,
    () => {
      fft.setInput(audio);
      amp.setInput(audio);
    },
    (err) => {
      console.error('Error cargando audio', err);
    }
  );
}

function togglePlay() {
  if (!audio) {
    alert('Carga un archivo de audio primero.');
    return;
  }
  if (audio.isPlaying()) {
    audio.pause();
    playBtn.html('▶️ Play');
  } else {
    audio.play();
    playBtn.html('⏸ Pausar');
  }
}

// -------------------- SPIDER --------------------
class Spider {
  constructor() {
    this.pos = p5.Vector.add(center, p5.Vector.random2D().mult(random(20, baseWeb.radius * 0.8)));
    this.prevPos = this.pos.copy();
    this.vel = p5.Vector.random2D().mult(random(0.8, 2));
    this.size = random(4, 9);
    this.changeTimer = millis() + random(1000, 4000);
  }

  update(dt) {
    this.prevPos = this.pos.copy();

    if (millis() > this.changeTimer) {
      this.vel = p5.Vector.random2D().mult(random(0.8, 2));
      this.changeTimer = millis() + random(1000, 4000);
    }

    // movimiento controlado por velocidad global
    this.pos.add(p5.Vector.mult(this.vel, dt * spiderSpeed));

    // mantener dentro del radio
    let maxR = baseWeb.radius * 1.0;
    let d = p5.Vector.sub(this.pos, center);
    if (d.mag() > maxR) {
      d.setMag(maxR);
      this.vel.mult(-1);
      this.pos = p5.Vector.add(center, d);
    }

    // trazo más traslúcido (menor alpha base)
    trailSegments.push({
      a: this.prevPos.copy(),
      b: this.pos.copy(),
      baseW: random(0.8, 1.8),
      baseA: 80, // antes 180 → más traslúcido
      phase: random(TWO_PI)
    });

    if (trailSegments.length > 4000) trailSegments.splice(0, 500);
  }

  draw() {
    fill(255);
    noStroke();
    circle(this.pos.x, this.pos.y, this.size + beatSmooth * 8);

    stroke(255, 255, 255, 160);
    strokeWeight(1 + beatSmooth * 2);
    for (let i = 0; i < 6; i++) {
      let a = p5.Vector.fromAngle(i * PI / 3 + frameCount * 0.01)
        .mult(this.size * 1.4)
        .add(this.pos);
      line(this.pos.x, this.pos.y, a.x, a.y);
    }
  }
}

// -------------------- DRAW --------------------
let lastTime = 0;
function draw() {
  let now = millis();
  let dt = constrain((now - lastTime) / 16.6, 0.1, 2);
  lastTime = now;

  background(8, 8, 10);

  let bassRaw = 0;
  let ampLevel = 0;
  if (audio && audio.isPlaying()) {
    bassRaw = fft.getEnergy('bass');
    ampLevel = amp.getLevel();
  } else {
    bassRaw = 60 + 50 * sin(millis() / 600);
    ampLevel = 0.15 + 0.05 * sin(millis() / 700);
  }

  let energy = constrain(bassRaw / 255, 0, 1);
  let delta = max(0, energy - prevEnergy);
  prevEnergy = lerp(prevEnergy, energy, 0.3);

  let sens = parseFloat(sensitivitySlider.value());
  let rawBeat = (energy * 2.5) + (delta * 8.0) + (ampLevel * 1.5);
  rawBeat *= sens;
  beatSmooth = lerp(beatSmooth, rawBeat, 0.65);
  beatSmooth = constrain(beatSmooth, 0, maxBeat);

  drawWeb(1 + beatSmooth * 0.18);

  for (let s of spiders) {
    s.update(dt);
    s.draw();
  }

  // --- Estelas más etéreas y brillantes ---
  push();
  blendMode(ADD);
  for (let t of trailSegments) {
    let osc = 0.5 + 0.5 * sin(millis() / 140 + t.phase);
    let a = t.baseA * (0.3 + beatSmooth * 1.2 * osc); // menor alpha global
    let w = t.baseW * (1 + 4 * beatSmooth * osc);
    stroke(currentColor[0], currentColor[1], currentColor[2], constrain(a, 5, 160));
    strokeWeight(constrain(w, 0.4, 10));
    line(t.a.x, t.a.y, t.b.x, t.b.y);
  }
  pop();

  noFill();
  stroke(255, 255, 255, 40 + beatSmooth * 40);
  strokeWeight(2 + beatSmooth * 1.6);
  circle(center.x, center.y, baseWeb.radius * 0.4 + beatSmooth * 100);
}

// ------------------ WEB ------------------
function drawWeb(scaleFactor = 1) {
  let col = webColors[colorIndex % webColors.length];
  let alpha = constrain(col[3] * (0.5 + 1.4 * beatSmooth), 30, 255);
  stroke(col[0], col[1], col[2], alpha);
  strokeWeight(1.2 + beatSmooth * 2.4);
  noFill();

  let spokes = baseWeb.spokes;
  let rings = baseWeb.rings;
  let rStep = (baseWeb.radius * scaleFactor) / rings;

  for (let s = 0; s < spokes; s++) {
    let theta = map(s, 0, spokes, 0, TWO_PI);
    let offset = sin(frameCount * 0.03 + s) * beatSmooth * 14;
    let x = center.x + (baseWeb.radius * scaleFactor + offset) * cos(theta);
    let y = center.y + (baseWeb.radius * scaleFactor + offset) * sin(theta);
    line(center.x, center.y, x, y);
  }

  for (let i = 0; i <= rings; i++) {
    beginShape();
    let r = rStep * i;
    for (let s = 0; s < spokes; s++) {
      let theta = map(s, 0, spokes, 0, TWO_PI);
      let noiseOffset = noise(s * 0.2, frameCount * 0.01);
      let rr = r + sin(theta * 3 + frameCount * 0.02) * beatSmooth * 12 + noiseOffset * 6;
      let x = center.x + rr * cos(theta);
      let y = center.y + rr * sin(theta);
      vertex(x, y);
    }
    endShape(CLOSE);
  }
}

// ------------------ INPUT ------------------
function keyPressed() {
  if (key === 'c' || key === 'C') {
    colorIndex = (colorIndex + 1) % webColors.length;
    currentColor = webColors[colorIndex].slice();
  }
  if (key === 'r' || key === 'R') {
    trailSegments = [];
  }
}

function mousePressed() {
  userStartAudio();
}
```

### Obra:

https://editor.p5js.org/ZombieYuu/sketches/rvu_KaXw-


https://github.com/user-attachments/assets/518fc116-bd18-498c-a03d-542d2aaa91da

## Auto evaluación:

Mi auto evaluación es 5.0, pues entregue todas las actividades satisfactoriamente.

