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
