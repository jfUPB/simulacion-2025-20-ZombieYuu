# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1: 

**¿Cómo funciona la suma dos vectores en p5.js?**
- Para sumar vectores tienes que usar ``add``, por ejemplo, ``position.add(velocity)`` .

**¿Por qué esta línea position = position + velocity; no funciona?**
- Esta linea no funciona porque el símbolo + no esta sobrecargado para sumar vectores, este se reemplaza por ``add``.

  ### Actividad 2:

  **¿Qué tuviste que hacer para hacer la conversión propuesta?**
- Agregué (position) ``pos`` a ``this`` entonces queda ``this.pos``, también se le cambia en donde dice ``this.x`` y ``this.y``.

  **Muestra el código que utilizaste para resolver el ejercicio.**

```javascript

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
this.pos = createVector(width /  2, height / 2)
  }

  show() {
    stroke(0);
    point(this.pos.x, this.pos.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.pos.x++;
    } else if (choice == 1) {
      this.pos.x--;
    } else if (choice == 2) {
      this.pos.y++;
    } else {
      this.pos.y--;
    }
  }
}
```

### Actividad 3:

**¿Qué resultado esperas obtener en el programa anterior?**
- Espero ver las coordenadas de los dos vectores en la consola.

**¿Qué resultado obtuviste?**
- Obtuve el resultado esperado.

**Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.**
- Cuando `` posicion = createVector`` se pasa al browser como vector mientras que `` playingVector(v)`` genera una copia de la dirección.

**¿Qué tipo de paso se está realizando en el código?**
- Es un paso de referencia cuando se pasa por `` playingVector(v)`` se modifica el vector

**¿Qué aprendiste?**
- Que los objetos que pasan por una referencoa pueden modificarse desde la función lo que afecta al objeto original.

### Actividad 4: 


### Actividad 5:

**El código que genera el resultado que te pedí**

```javascript
let Interpolation = 0;
let step = 0.01;


function setup() {
    createCanvas(300, 300);
}

function draw() {
    background(200);
  
  Interpolation += step;
  
  if (Interpolation > 1 || Interpolation < 0){
    step *= -1;
    
  }

    let v0 = createVector(50, 50);
    let v1 = createVector(200, 0);
    let v2 = createVector(0, 200);
    let v3 = p5.Vector.lerp(v1, v2, Interpolation);
    let v4 = p5.Vector.sub(v2, v1)  
    let Color = lerpColor( 'red',  'blue', Interpolation)
    
    
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, Color);
    drawArrow(p5.Vector.add(v0, v1), v4, 'green' )
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```

**¿Cómo funciona lerp() y lerpColor()?**

- ``lerp()`` tiene la función de interpolar dos vectores y ``lerpColor()`` se encarga de interpolar dos colores dando el efecto de degradado

**¿Cómo se dibuja una flecha usando drawArrow()?** 

- Se coloca la direccion de inicio y el fin del vector, por ejemplo: `` drawArrow(v0, v1, 'red')``
