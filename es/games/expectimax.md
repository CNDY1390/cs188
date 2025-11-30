---
title: 3.3 Expectimax
parent: 3. Juegos
nav_order: 3
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 3.3 Expectimax

Ahora hemos visto cómo funciona minimax y cómo ejecutar minimax completo nos permite responder de manera óptima contra un oponente óptimo. Sin embargo, minimax tiene algunas restricciones naturales sobre las situaciones a las que puede responder. Debido a que minimax cree que está respondiendo a un oponente óptimo, a menudo es demasiado pesimista en situaciones donde no se garantizan respuestas óptimas a las acciones de un agente. Tales situaciones incluyen escenarios con aleatoriedad inherente, como juegos de cartas o dados, u oponentes impredecibles que se mueven aleatoriamente o de manera subóptima. Hablaremos sobre escenarios con aleatoriedad inherente con mucho más detalle cuando discutamos los **procesos de decisión de Markov** en la segunda mitad del curso.

Esta aleatoriedad se puede representar a través de una generalización de minimax conocida como **expectimax**. Expectimax introduce *nodos de azar* en el árbol de juego, que en lugar de considerar el peor de los casos como lo hacen los nodos minimizadores, considera el *caso promedio*. Más específicamente, mientras que los minimizadores simplemente calculan la utilidad mínima sobre sus hijos, los nodos de azar calculan la **utilidad esperada** o el valor esperado. Nuestra regla para determinar los valores de los nodos con expectimax es la siguiente:

$$\forall \text{estados controlados por el agente}, V(s) = \max_{s'\in sucesores(s)}V(s')$$

$$\forall \text{estados de azar}, V(s) = \sum_{s'\in sucesores(s)}p(s'|s)V(s')$$

$$\forall \text{estados terminales}, V(s) = \text{conocido}$$

En la formulación anterior, $$p(s'|s)$$ se refiere a la probabilidad de que una acción no determinista dada resulte en pasar del estado $$s$$ a $$s'$$, o la probabilidad de que un oponente elija una acción que resulte en pasar del estado $$s$$ a $$s'$$, dependiendo de los detalles del juego y del árbol de juego bajo consideración. A partir de esta definición, podemos ver que minimax es simplemente un caso especial de expectimax. Los nodos minimizadores son simplemente nodos de azar que asignan una probabilidad de 1 a su hijo de menor valor y una probabilidad de 0 a todos los demás hijos. En general, las probabilidades se seleccionan para reflejar adecuadamente el estado del juego que estamos tratando de modelar, pero cubriremos cómo funciona este proceso con más detalle en notas futuras. Por ahora, es justo suponer que estas probabilidades son simplemente propiedades inherentes del juego.

El pseudocódigo para expectimax es bastante similar a minimax, con solo unos pequeños ajustes para tener en cuenta la utilidad esperada en lugar de la utilidad mínima, ya que estamos reemplazando los nodos minimizadores con nodos de azar:

<img src="{{ site.baseurl }}/assets/images/expectimax-pseudocode.png" alt="Pseudocódigo Expectimax" />

Antes de continuar, repasemos rápidamente un ejemplo simple. Considere el siguiente árbol expectimax, donde los nodos de azar están representados por nodos circulares en lugar de los triángulos orientados hacia arriba/abajo para maximizadores/minimizadores.

<img src="{{ site.baseurl }}/assets/images/unfilled-expectimax.png" alt="Expectimax sin rellenar" />

Supongamos por simplicidad que todos los hijos de cada nodo de azar tienen una probabilidad de ocurrencia de $$\frac{1}{3}$$. Por lo tanto, a partir de nuestra regla expectimax para la determinación del valor, vemos que de izquierda a derecha los 3 nodos de azar toman valores de $$\frac{1}{3} \cdot 3 + \frac{1}{3} \cdot 12 + \frac{1}{3} \cdot 9 = \boxed{8}$$, $$\frac{1}{3} \cdot 2 + \frac{1}{3} \cdot 4 + \frac{1}{3} \cdot 6 = \boxed{4}$$, y $$\frac{1}{3} \cdot 15 + \frac{1}{3} \cdot 6 + \frac{1}{3} \cdot 0 = \boxed{7}$$. El maximizador selecciona el máximo de estos tres valores, $$\boxed{8}$$, produciendo un árbol de juego completo de la siguiente manera:

<img src="{{ site.baseurl }}/assets/images/filled-expectimax.png" alt="Expectimax relleno" />

Como nota final sobre expectimax, es importante darse cuenta de que, en general, es necesario mirar a todos los hijos de los nodos de azar: no podemos podar de la misma manera que podríamos para minimax. A diferencia de cuando se calculan mínimos o máximos en minimax, un solo valor puede sesgar el valor esperado calculado por expectimax arbitrariamente alto o bajo. Sin embargo, la poda puede ser posible cuando tenemos límites finitos conocidos sobre los valores posibles de los nodos.

## 3.3.1 Tipos de capas mixtas

Aunque minimax y expectimax requieren alternar nodos maximizadores/minimizadores y nodos maximizadores/azar respectivamente, muchos juegos aún no siguen el patrón exacto de alternancia que exigen estos dos algoritmos. Incluso en Pacman, después de que Pacman se mueve, generalmente hay múltiples fantasmas que se turnan para hacer movimientos, no un solo fantasma. Podemos dar cuenta de esto agregando capas de manera muy fluida en nuestros árboles de juego según sea necesario. En el ejemplo de Pacman para un juego con cuatro fantasmas, esto se puede hacer teniendo una capa maximizadora seguida de 4 capas consecutivas de fantasma/minimizador antes de la segunda capa de Pacman/maximizador. De hecho, hacerlo da lugar inherentemente a la cooperación entre todos los minimizadores, ya que alternativamente se turnan para minimizar aún más la utilidad alcanzable por el(los) maximizador(es). Incluso es posible combinar capas de nodos de azar con minimizadores y maximizadores. Si tenemos un juego de Pacman con dos fantasmas, donde un fantasma se comporta aleatoriamente y el otro se comporta de manera óptima, podríamos simular esto con grupos alternos de nodos maximizador-azar-minimizador.

<img src="{{ site.baseurl }}/assets/images/mixed-layer.png" alt="Capa mixta" />

Como es evidente, hay bastante espacio para una variación robusta en la estratificación de nodos, lo que permite el desarrollo de árboles de juego y algoritmos de búsqueda adversaria que son híbridos modificados de expectimax/minimax para cualquier juego de suma cero.
