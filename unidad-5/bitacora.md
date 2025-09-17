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

    <img width="898" height="773" alt="image" src="https://github.com/user-attachments/assets/79d31e31-45cd-4ffe-a50f-529ee683e796" />


### Ejemplo 4.4: A system of systems

- Se generan varios sistemas de partículas de forma independiente.
- Se matan las partículas mas viejas de cada sistema.
- Se crea un ``array`` para cada sistema para eliminar las partículas muertas.

  ### Obra generativa 2:
  -La gestión de partículas es diferente, al inicio se crean las partículas normalmente, luego las viejas se guardan en ``pool``, y cuando necesito nuevas las reciclo de ahí y cuando hay demaciadas partículas no permito que se creen mas.
  -Tomando como base *A system of systems* utilicé el concepto de los N Cuerpos de la unidad 3 y el random de la unidad 1 para crear la obra que sigue el mouse como punto gravitacional despues de ser generado.
  - https://editor.p5js.org/ZombieYuu/sketches/AC5QhH7P1

  <img width="907" height="717" alt="image" src="https://github.com/user-attachments/assets/1b96b5b9-bc6f-4b71-88a1-c4471ee8c835" />


### Ejemplo 4.5: a Particle System with Inheritance and Polymorphism

- En este caso tenemos dor partículas diferentes, ``Particle`` y ``Confetti`` que estan en la misma clase ``this.particles.push``, lo que significa que existe un enparentamiento y polimorfismo.
- Las dos distintas partículas se matan de igual forma.
- Y Las dos partículas llegan a un mismo ``array`` y se elimina.

  ### Obra generativa 3:
  - Primero, las partículas nacen en el extremo del péndulo y van viviendo mientras caen y se van apagando poco a poco. Cuando su tiempo se acaba, las quito de la lista y desaparecen
  - En base a * Particle System with Inheritance and Polymorphism* quise simular una cascada de hielos y gotas de aguas en un pendulo, concepto de la unidad 4 asi cuando el pendulo se mueve la cascada también.
  - https://editor.p5js.org/ZombieYuu/sketches/GLoLgAnbG
 
    <img width="863" height="595" alt="image" src="https://github.com/user-attachments/assets/0997af0c-5963-4871-ae0d-782937806d99" />


### Ejemplo 4.6:  a Particle System with Forces

- Las partículas se crean con ``add.Particle`` y pueden ser instancias de distintos tipos por las clases derivadas.
- Las particulas se matan de la misma manera.
- Se aplican fuerzas como la gravedad, pero no afecta en la gestión de la memoria, así mismo se maneja de la misma forma que los ejemplos anteriores.

  ### Obra generativa 4:
  - La estructura de gestion de partículas es similar a la que ya hemos visto antes, se crea una partícula, despues muere y se borra de la lista.
  - Con base en *a Particle System with Forces* utilicé el concepto de motion 101 de la unidad 2 y el random para tener diversos colores, con la letra C se borra la pantalla y con la R se agrega otra posición de emición.
  - https://editor.p5js.org/ZombieYuu/sketches/TvuMniRMM
 
    <img width="898" height="755" alt="image" src="https://github.com/user-attachments/assets/a1c800aa-56f0-4800-9e9c-291400a5e664" />


### Ejemplo 4.7: a Particle System with a Repeller

- La creación de partículas funciona de la misma forma que en los ejemplos anteriores.
- La muerte de las partículas es igual.
- Se agrega una nueva fuerza de repulción, no afecta a la gestión de memoria pero las partículas muertas se eliminan por medio de ``Particles.remove(i)``.

  ### Obra generativa 5:
  - La gestión de partículas es igual que la anterior obra generativa, pero aquí la cantidad de partículas limita hasta 1020.
  - En base a *a Particle System with a Repeller* utilicé conceptos como el ruido de perlin y la atracción gravitacional para crear la obra.
  - https://editor.p5js.org/ZombieYuu/sketches/MfuVgbPxf
 
    <img width="917" height="770" alt="image" src="https://github.com/user-attachments/assets/2e0491c3-2571-44ba-9b54-a9668350c3b8" />






