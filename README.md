
### MEJORAS Y CORRECCIONES PC4
### Nombre del problema: Tower Defense

Para la implementacion inicial no cambie mucho respecto a lo enviado sin embargo hice unas pequeñas modificaciones en la clase `Game`:

#### Cambio en la inicialización del mapa:

- Antes: this.map = new Map();
- Ahora: this.map = new Map(mapRows, mapCols);
Esto se hizo para corregir el error de instanciación de la clase abstracta Map.
![alt text](image-39.png)

#### Adición del método initializeMap:

Se agregó para inicializar el mapa con un diseño predefinido.

![alt text](image-40.png)

#### Ajuste del método placeTower:

Anteriormente, la colocación de torres y su validación se hacían directamente.
Ahora se utiliza createTower para crear instancias de torres según su tipo.

![alt text](image-41.png)
#### Adición del método startNextWave:

Se ajustó para inicializar una nueva oleada y mostrar información sobre los enemigos.
![alt text](image-42.png)
#### Adición del método displayGameState:

Se agregó para mostrar el estado actual del juego, incluyendo la puntuación y la vida de la base.

![alt text](image-43.png)

#### Adición del método simulateGame:

Se agregó un bucle para permitir la entrada del usuario y simular el juego, manejando acciones como colocar torres e iniciar oleadas.

![alt text](image-44.png)

Ahora la compilacion se ve asi:

![alt text](image-45.png)

Estos cambios se realizaron para poder hacer las pruebas:
### Implementación de pruebas
#### Mocks:

- Utiliza Mockito para crear mocks de las clases Enemy y Tower para verificar la interacción
entre objetos.

Primero agregamos la dependencia para usar mockito al buildgradle de nuestro proyecto: ![alt text](image-23.png)

Recordemos que un stub se utiliza para simular el comportamiento de componentes dependientes (en este caso, enemigos y torres) con el fin de aislar la lógica que queremos probar.

Ahora escribimos la prueba:

![alt text](image-46.png)

Este test:

- Crea instancias simuladas de Tower y Enemy.
- Configura estas instancias para devolver valores específicos cuando se llamen a ciertos métodos.
- Simula un ataque donde la torre inflige daño al enemigo.
- Verifica que los métodos de la torre y el enemigo se llamaron el número correcto de veces.
- Verifica que el daño infligido y la salud restante del enemigo son los valores esperados.
- Esto asegura que la lógica básica de aplicar daño de una torre a un enemigo se comporta correctamente en términos de las interacciones y el cálculo de salud restante.


#### Stubs:
- Crea stubs para métodos que devuelven enemigos o torres específicos.

Nuestro objetivo es probar la generación de enemigos en una oleada (Wave) específica.

Ejemplo con Torre (Tower)

Definición de la Interfaz y la Implementación de la Fábrica de Torres (TowerFactory y CannonTowerFactory):

Creo la interfaz:

![alt text](image-49.png)

En este caso, CannonTowerFactory implementa TowerFactory y tiene un método createTower que crea una torre específica (CannonTower) según el tipo especificado ("Cannon" en este caso).

![alt text](image-50.png)

Ahora necesito escribir una prueba unitaria utilizando un stub (CannonTowerFactoryStub) en lugar de la implementación real (CannonTowerFactory) para asegurarnos de que estamos probando solo la lógica que nos interesa y no el comportamiento real de la fábrica.

![alt text](image-51.png)

En la prueba, se utiliza CannonTowerFactoryStub en lugar de CannonTowerFactory. Se llama al método createTower("Cannon") en towerFactory, que debería devolver una instancia de CannonTower (gracias al stub). Luego, verificamos que los atributos de la torre (damage, range, fireRate) coinciden con los valores esperados.

![alt text](image-52.png)

#### El mismo principio se aplica para el caso para probar enemigos usando stubs:
![alt text](image-53.png)

y

![alt text](image-54.png)

Ahora se escribira una prueba unitaria que utiliza un stub (BasicEnemyFactoryStub) para simular la creación de enemigos. Esta prueba verificara que los atributos (speed, health, reward) de un enemigo creado coincidan con los valores esperados.

![alt text](image-55.png)

En la prueba, utilizamos BasicEnemyFactoryStub en lugar de BasicEnemyFactory. Llamamos al método createEnemy("Basic") en enemyFactory, que debería devolver una instancia de BasicEnemy (gracias al stub). Luego, verificamos que los atributos del enemigo (speed, health, reward) coincidan con los valores esperados.


#### Fakes:

- Utilizar fakes para simular la base de datos de puntuación o estados de oleadas.

Implementare un fake simple para simular la base de datos de puntuación del jugador.
Vamos a crear clases que simulen la base de datos de puntuación y los estados de oleada

##### Fake para la Base de Datos de Puntuación: 

![alt text](image-56.png)

![alt text](image-57.png)

##### Fake para Estados de Oleadas: 

![alt text](image-58.png)


![alt text](image-59.png)

Ahora vamos a escribir una prueba unitaria que utilice estos fakes para simular el comportamiento de la base de datos de puntuación y los estados de oleadas.

![alt text](image-60.png)

- Prueba testFakeScoreDatabase: Utiliza FakeScoreDatabase para simular el almacenamiento y recuperación de puntajes. Primero, guardamos un puntaje de 1000 y luego verificamos que se haya guardado correctamente llamando a getScore().

- Prueba testFakeWaveState: Utiliza FakeWaveState para simular el seguimiento de los estados de oleadas en el juego. Verificamos inicialmente que la oleada actual sea la número 1. Luego, avanzamos a la siguiente oleada llamando a advanceToNextWave() y verificamos que ahora la oleada actual sea la número 2.

![alt text](image-63.png)

#### Pruebas de mutación:
- Implementa pruebas de mutación para verificar la calidad de las pruebas unitarias.
- ¿Qué herramienta utilizarías para realizar pruebas de mutación en este proyecto, y cómo la
configurarías?


En esta parte de igual manera que la anterior se usara pitest, con una correcion, pues en el avance de la pc4 mi cobertura era de 0 % pero era debido a que no compilaba mis test antes de el reporte para solucionar eso se agrega la siguiente linea en el gradle:

![alt text](image-61.png)

Entonces ahora ejecuto con el comando `./gradlew pitest` y abrimos el reporte:

![alt text](image-62.png)


 Aunque mis pruebas tienen un buen nivel de fuerza (100%, es decir que todas las pruebas pasan correctamente), la cobertura de mutación del 18% sugiere que podría haber áreas en las cuales las mutaciones introducidas no están siendo suficientemente probadas. Esto es cierto pues se ha escrito pruebas solo para algunas clases, sin embargo podemos aunmentar la cobertura revisando las mutaciones que no fueron detectadas y ajustar las pruebas para capturar estos casos.

![alt text](image-64.png)

Por ejemplo hare un test para Map puesto que no hemos cbuierto esa clase:

![alt text](image-65.png)

Y si ahora ejecuto el pitest:

![alt text](image-66.png)

Aumento a 25%, dado que el objetivo no es hacer pruebas unitarias para cada clase si no mas bien mostrar ejemplos de mocks,stubs y fakes, podemos decir que nuestra cobertura de mutaciones es la correcta.