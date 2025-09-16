# Evidencias de la unidad 5


## Actividad 2:

### Ejemplo 4.2:  an Array of Particles

- Con la siguiente instancia `` particles.push(new Particle(width / 2, 20));`` se crea cada partícula por cada frame.
- La eliminación de partículas se elabora al "revés" pues se "matan" las partículas mas viejas.
- La memoria se gestiona por medio de un ``array`` con el método de ``run`` lo que genera un loop en el ``arraylist`` y elimina las partículas muertas.

  ### Fuego pirotécnico:
  - En la grstión de creación y desaparición de partículas, creo partículas nuevas, las dejo vivir un tiempo, y cuando mueren las borro de la lista para que no ocupen memoria.
  - En la bace de *Array of particles* utilicé los conceptos de distribución no uniforme y el random para cambiar el color de las partículas con la tecla *c*. Generando así una simulación de juegos artificiales.
  - https://editor.p5js.org/ZombieYuu/sketches/kPezPX3sh

    <img width="898" height="773" alt="image" src="https://github.com/user-attachments/assets/79d31e31-45cd-4ffe-a50f-529ee683e796" />


### Ejemplo 4.4: A system of systems

- Se generan varios sistemas de partículas de forma independiente.
- Se matan las partículas mas viejas de cada sistema.
- Se crea un ``array`` para cada sistema para eliminar las partículas muertas.

### Ejemplo 4.5: a Particle System with Inheritance and Polymorphism

- En este caso tenemos dor partículas diferentes, ``Particle`` y ``Confetti`` que estan en la misma clase ``this.particles.push``, lo que significa que existe un enparentamiento y polimorfismo.
- Las dos distintas partículas se matan de igual forma.
- Y Las dos partículas llegan a un mismo ``array`` y se elimina.

### Ejemplo 4.6:  a Particle System with Forces

- Las partículas se crean con ``add.Particle`` y pueden ser instancias de distintos tipos por las clases derivadas.
- Las particulas se matan de la misma manera.
- Se aplican fuerzas como la gravedad, pero no afecta en la gestión de la memoria, así mismo se maneja de la misma forma que los ejemplos anteriores.

### Ejemplo 4.7: a Particle System with a Repeller

- La creación de partículas funciona de la misma forma que en los ejemplos anteriores.
- La muerte de las partículas es igual.
- Se agrega una nueva fuerza de repulción, no afecta a la gestión de memoria pero las partículas muertas se eliminan por medio de ``Particles.remove(i)``.




