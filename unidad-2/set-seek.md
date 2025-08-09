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

**¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?** 

-   El métodp ``mag() `` es para hayar la magnitud o longitud de un vector, mientras que  `` magSq()`` convierte no utiliza la raíz cuadrada al hayar la magnitud, siendo mas eficiente.

**¿Para qué sirve el método normalize()?**

- El método  `` normalize()`` sirve para convertir el vector en uno unitario, o sea convertir la magnitud en 1 si solo se nececita la dirección.

**Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?**

-  `` dot()`` es para medir a donde apuntan dos vectores en la misma dirección.

**El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?**

- La versión estática se llama desde una clase y utiliza dos vectores como argumento y la versión de instancia llama fuera de una clase a un vector y se le pasa a otro.

**Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.**

- El producto cruz genera un nuevo vector perpendicular a los dos vectores originales, mientras que la orientación apunta hacia afuera de la superficie, del primer vector hacia el segundo, la magnitud representa el area que forman los dos vectores.

**¿Para que te puede servir el método dist()?**

- El método  `` dist()`` sirve para hayar la distancia entre dos vectores.

**¿Para qué sirven los métodos normalize() y limit()?**

- `` normalize()`` al convertir un vector a unitario sirve para mantener la dirección constante sin importar la fuerza,  `` limit()`` restringe la magnitud de un vector a un máximo que se elije y sirve para controlar la veklocidad máxima.


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

  ### Actividad 6:

**Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.**

-Es un concepto para simular el movimiento de un objeto, utilizando los conceptos de velocidad, aceleración y dirección.

**¿Cómo se aplica motion 101 en el ejemplo?**

- Se aplica creando un objeto con atributos: *position*, *velocity*, *aceleration*. 
- Se aplica una aceleración.
- Se actualiza la velocidad con dicha aceleración.
- Se aplica la nueva velocidad a la posición.

  ### Actividad 7:

- **Aceleración constante:** El objeto aumenta la velocidad de manera constante en una sola dirección.
- **Aceleración aleatoria:** El objeto acelera y pausa de manera aleatoria e impredecible, haciendolo moverse en varias direcciciones y con velocidades diferentes.
- **Aceleración hacia el mouse:** El objeto mientras más lejos del mouse aumenta su aceleración para llegar a el.
