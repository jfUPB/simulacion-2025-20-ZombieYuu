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

En este experimento utilicé el ``MouseConstraint``, un mundo con gravedad e inclui dos objetos simples como un triangulko y un círculo.

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

  

