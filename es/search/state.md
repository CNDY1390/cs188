---
title: 1.2 Espacios de estados y problemas de búsqueda
parent: 1. Búsqueda
nav_order: 2
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 1.2 Espacios de estados y problemas de búsqueda

Para crear un agente de planificación racional, necesitamos una forma de expresar matemáticamente el entorno dado en el que existirá el agente. Para hacer esto, debemos expresar formalmente un **problema de búsqueda**: dado el **estado** actual de nuestro agente (su configuración dentro de su entorno), ¿cómo podemos llegar a un nuevo estado que satisfaga sus objetivos de la mejor manera posible? Un problema de búsqueda consta de los siguientes elementos:

- Un **espacio de estados**: el conjunto de todos los estados posibles que son posibles en su mundo dado
- Un conjunto de **acciones** disponibles en cada estado
- Un modelo de **transición**: genera el siguiente estado cuando se toma una acción específica en el estado actual
- Un **costo** de acción: incurrido al pasar de un estado a otro después de aplicar una acción
- Un **estado inicial**: el estado en el que existe un agente inicialmente
- Una **prueba de objetivo**: una función que toma un estado como entrada y determina si es un estado objetivo

Fundamentalmente, un problema de búsqueda se resuelve considerando primero el estado inicial, luego explorando el espacio de estados utilizando los métodos de acción, transición y costo, calculando iterativamente los hijos de varios estados hasta llegar a un estado objetivo, momento en el cual habremos determinado un camino desde el estado inicial hasta el estado objetivo (típicamente llamado **plan**). El orden en el que se consideran los estados se determina utilizando una **estrategia** predeterminada. Cubriremos los tipos de estrategias y su utilidad en breve.

Antes de continuar con cómo resolver problemas de búsqueda, es importante notar la diferencia entre un **estado del mundo** y un **estado de búsqueda**. Un estado del mundo contiene toda la información sobre un estado dado, mientras que un estado de búsqueda contiene solo la información sobre el mundo que es necesaria para la planificación (principalmente por razones de eficiencia espacial). Para ilustrar estos conceptos, presentaremos el ejemplo motivador distintivo de este curso: Pacman. El juego de Pacman es simple: Pacman debe navegar por un laberinto y comer todas las (pequeñas) bolitas de comida en el laberinto sin ser comido por los fantasmas patrulleros maliciosos. Si Pacman come una de las (grandes) bolitas de poder, se vuelve inmune a los fantasmas por un período de tiempo establecido y gana la capacidad de comer fantasmas por puntos.

<img src="{{ site.baseurl }}/assets/images/pacman_example.png" alt="Imagen de ejemplo de Pacman" />

Consideremos una variación del juego en la que el laberinto contiene solo a Pacman y bolitas de comida. Podemos plantear dos problemas de búsqueda distintos en este escenario: búsqueda de caminos y comer todos los puntos. La búsqueda de caminos intenta resolver el problema de llegar de la posición $$(x_1, y_1)$$ a la posición $$(x_2, y_2)$$ en el laberinto de manera óptima, mientras que comer todos los puntos intenta resolver el problema de consumir todas las bolitas de comida en el laberinto en el menor tiempo posible. A continuación, se enumeran los estados, acciones, modelo de transición y prueba de objetivo para ambos problemas:

**Búsqueda de caminos (Pathing)**
- Estados: ubicaciones $$(x,y)$$
- Acciones: Norte, Sur, Este, Oeste
- Modelo de transición (obtener el siguiente estado): Actualizar solo la ubicación
- Prueba de objetivo: ¿Es $$(x,y)=FIN$$?

**Comer todos los puntos (Eat-all-dots)**
- Estados: {ubicación $$(x,y)$$, booleanos de puntos}
- Acciones: Norte, Sur, Este, Oeste
- Modelo de transición (obtener el siguiente estado): Actualizar ubicación y booleanos
- Prueba de objetivo: ¿Son falsos todos los booleanos de puntos?

Tenga en cuenta que para la búsqueda de caminos, los estados contienen menos información que los estados para comer todos los puntos, porque para comer todos los puntos debemos mantener una matriz de booleanos correspondientes a cada bolita de comida y si se ha comido o no en el estado dado. Un estado del mundo puede contener aún más información, codificando potencialmente información sobre cosas como la distancia total recorrida por Pacman o todas las posiciones visitadas por Pacman además de su ubicación actual $$(x,y)$$ y booleanos de puntos.

## 1.2.1 Tamaño del espacio de estados

Una pregunta importante que surge a menudo al estimar el tiempo de ejecución computacional para resolver un problema de búsqueda es el tamaño del espacio de estados. Esto se hace casi exclusivamente con el **principio fundamental de conteo**, que establece que si hay n objetos variables en un mundo dado que pueden tomar $$x_1$$, $$x_2$$, ..., $$x_n$$ valores diferentes respectivamente, entonces el número total de estados es $$x_1$$ · $$x_2$$ · ... · $$x_n$$. Usemos Pacman para mostrar este concepto con un ejemplo:

<img src="{{ site.baseurl }}/assets/images/state_space_size.png" alt="Tamaño del espacio de estados" />

Digamos que los objetos variables y su número correspondiente de posibilidades son los siguientes:
- *Posiciones de Pacman* - Pacman puede estar en 120 posiciones distintas ($$x$$,$$y$$), y solo hay un Pacman
- *Dirección de Pacman* - esto puede ser Norte, Sur, Este u Oeste, para un total de 4 posibilidades
- *Posiciones de fantasmas* - Hay dos fantasmas, cada uno de los cuales puede estar en 12 posiciones distintas ($$x$$,$$y$$)
- *Configuraciones de bolitas de comida* - Hay 30 bolitas de comida, cada una de las cuales puede ser comida o no comida

Usando el principio fundamental de conteo, tenemos 120 posiciones para Pacman, 4 direcciones en las que Pacman puede estar mirando, $$12 \cdot 12$$ configuraciones de fantasmas (12 para cada fantasma) y $$2 \cdot 2 \cdot \ldots \cdot 2 = 2^{30}$$ configuraciones de bolitas de comida (cada una de las 30 bolitas de comida tiene dos valores posibles: comida o no comida). Esto nos da un tamaño total del espacio de estados de **$$120 \cdot 4 \cdot 12^{2} · 2^{30}$$**.

## 1.2.2 Grafos de espacio de estados y árboles de búsqueda

Ahora que hemos establecido la idea de un espacio de estados y los cuatro componentes necesarios para definir uno completamente, estamos casi listos para comenzar a resolver problemas de búsqueda. La pieza final del rompecabezas es la de los grafos de espacio de estados y los árboles de búsqueda.

Recuerde que un grafo se define por un conjunto de nodos y un conjunto de bordes que conectan varios pares de nodos. Estos bordes también pueden tener pesos asociados con ellos. Un **grafo de espacio de estados** se construye con estados que representan nodos, con bordes dirigidos que existen desde un estado a sus hijos. Estos bordes representan acciones, y cualquier peso asociado representa el costo de realizar la acción correspondiente. Por lo general, los grafos de espacio de estados son demasiado grandes para almacenarlos en la memoria (¡incluso nuestro ejemplo simple de Pacman de arriba tiene $$\approx 10^{13}$$ estados posibles, ¡uf!), pero es bueno tenerlos en cuenta conceptualmente al resolver problemas. También es importante tener en cuenta que en un grafo de espacio de estados, cada estado se representa exactamente una vez: simplemente no hay necesidad de representar un estado varias veces, y saber esto ayuda bastante cuando se trata de razonar sobre problemas de búsqueda.

A diferencia de los grafos de espacio de estados, nuestra siguiente estructura de interés, los **árboles de búsqueda**, no tienen tal restricción sobre el número de veces que puede aparecer un estado. Esto se debe a que, aunque los árboles de búsqueda también son una clase de grafo con estados como nodos y acciones como bordes entre estados, cada estado/nodo codifica no solo el estado en sí, sino todo el camino (o **plan**) desde el estado inicial hasta el estado dado en el grafo de espacio de estados. Observe el grafo de espacio de estados y el árbol de búsqueda correspondiente a continuación:

<img src="{{ site.baseurl }}/assets/images/graph_and_tree.png" alt="Grafo y árbol" />

El camino resaltado (S → d → e → r → f → G) en el grafo de espacio de estados dado se representa en el árbol de búsqueda correspondiente siguiendo el camino en el árbol desde el estado inicial S hasta el estado objetivo resaltado G. De manera similar, todos y cada uno de los caminos desde el nodo inicial a cualquier otro nodo se representan en el árbol de búsqueda mediante un camino desde la raíz S hasta algún descendiente de la raíz correspondiente al otro nodo. Dado que a menudo existen múltiples formas de llegar de un estado a otro, los estados tienden a aparecer varias veces en los árboles de búsqueda. Como resultado, los árboles de búsqueda son mayores o iguales a su grafo de espacio de estados correspondiente en tamaño.

Ya hemos determinado que los grafos de espacio de estados en sí mismos pueden ser enormes en tamaño incluso para problemas simples, por lo que surge la pregunta: ¿cómo podemos realizar cálculos útiles en estas estructuras si son demasiado grandes para representarlas en la memoria? La respuesta radica en cómo calculamos los hijos de un estado actual: solo almacenamos los estados con los que estamos trabajando inmediatamente y calculamos los nuevos bajo demanda utilizando los métodos correspondientes getNextState, getAction y getActionCost. Por lo general, los problemas de búsqueda se resuelven utilizando árboles de búsqueda, donde almacenamos con mucho cuidado unos pocos nodos seleccionados para observar a la vez, reemplazando iterativamente los nodos con sus hijos hasta llegar a un estado objetivo. Existen varios métodos para decidir el orden en el que realizar este reemplazo iterativo de nodos del árbol de búsqueda, y presentaremos estos métodos ahora.
