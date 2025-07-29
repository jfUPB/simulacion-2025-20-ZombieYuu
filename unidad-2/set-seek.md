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

