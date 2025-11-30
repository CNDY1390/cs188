---
title: 7.2 Redes de decisión
parent: 7. Redes de decisión y VPIs
nav_order: 2
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 7.2 Redes de decisión

Anteriormente aprendimos sobre árboles de juegos y algoritmos como minimax y expectimax, que usamos para determinar acciones óptimas que maximizaran nuestra utilidad esperada. Luego, en la quinta nota, discutimos las redes bayesianas y cómo podemos usar la evidencia que conocemos para ejecutar inferencia probabilística para hacer predicciones. Ahora discutiremos una combinación de redes bayesianas y expectimax conocida como **red de decisión** que podemos usar para modelar el efecto de varias acciones en las utilidades basadas en un modelo probabilístico gráfico general. Sumerjámonos directamente en la anatomía de una red de decisión:

- **Nodos de azar**: los nodos de azar en una red de decisión se comportan de manera idéntica a las redes bayesianas. Cada resultado en un nodo de azar tiene una probabilidad asociada, que se puede determinar ejecutando inferencia en la red bayesiana subyacente a la que pertenece. Los representaremos con óvalos.
- **Nodos de acción**: los nodos de acción son nodos sobre los que tenemos control total; son nodos que representan una elección entre cualquiera de una serie de acciones entre las que tenemos el poder de elegir. Representaremos los nodos de acción con rectángulos.
- **Nodos de utilidad**: los nodos de utilidad son hijos de alguna combinación de nodos de acción y azar. Emiten una utilidad basada en los valores tomados por sus padres, y se representan como diamantes en nuestras redes de decisión.

Considere una situación en la que está decidiendo si llevar o no un paraguas cuando sale a clase por la mañana, y sabe que hay un pronóstico de 30% de probabilidad de lluvia. ¿Deberías llevar el paraguas? Si hubiera un 80% de probabilidad de lluvia, ¿cambiaría su respuesta? Esta situación es ideal para modelar con una red de decisión, y lo hacemos de la siguiente manera:

<img src="{{ site.baseurl }}/assets/images/dn-weather.png" alt="Ejemplo de clima de red de decisión" />

Como hemos hecho a lo largo de este curso con las diversas técnicas de modelado y algoritmos que hemos discutido, nuestro objetivo con las redes de decisión es nuevamente seleccionar la acción que produce la **utilidad máxima esperada** (MEU). Esto se puede hacer con un procedimiento bastante sencillo e intuitivo:

- Comience instanciando toda la evidencia que se conoce y ejecute la inferencia para calcular las probabilidades posteriores de todos los padres del nodo de azar del nodo de utilidad en el que se alimenta el nodo de acción.
- Revise cada acción posible y calcule la utilidad esperada de tomar esa acción dadas las probabilidades posteriores calculadas en el paso anterior. La utilidad esperada de tomar una acción $$a$$ dada la evidencia $$e$$ y $$n$$ nodos de azar se calcula con la siguiente fórmula:

  $$EU(a \mid e) = \sum_{x_1, ..., x_n}P(x_1, ..., x_n \mid e)U(a, x_1, ..., x_n)$$

  donde cada $$x_i$$ representa un valor que el $$i$$-ésimo nodo de azar puede tomar. Simplemente tomamos una suma ponderada sobre las utilidades de cada resultado bajo nuestra acción dada con pesos correspondientes a las probabilidades de cada resultado.
- Finalmente, seleccione la acción que arrojó la utilidad más alta para obtener la MEU.

Veamos cómo se ve esto realmente calculando la acción óptima (¿deberíamos *dejar* o *tomar* nuestro paraguas) para nuestro ejemplo meteorológico, utilizando tanto la tabla de probabilidad condicional para el clima dado un mal pronóstico del tiempo (el pronóstico es nuestra variable de evidencia) como la tabla de utilidad dada nuestra acción y el clima:

<img src="{{ site.baseurl }}/assets/images/dn-with-table.png" alt="Red de decisión con tabla" />

Tenga en cuenta que hemos omitido el cálculo de inferencia para las probabilidades posteriores $P(W \mid F = \text{malo})$, pero podríamos calcularlas utilizando cualquiera de los algoritmos de inferencia que discutimos para las redes bayesianas. En cambio, aquí simplemente asumimos la tabla anterior de probabilidades posteriores para $P(W \mid F = \text{malo})$ como dada. Revisar nuestras acciones y calcular las utilidades esperadas produce:

$$\begin{aligned}
EU(\text{dejar} \mid \text{malo}) &= \sum_{w}P(w \mid \text{malo})U(\text{dejar}, w) \\
                                &= 0.34 \cdot 100 + 0.66 \cdot 0 = \boxed{34} \\
EU(\text{tomar} \mid \text{malo}) &= \sum_{w}P(w \mid \text{malo})U(\text{tomar}, w) \\
                               &= 0.34 \cdot 20 + 0.66 \cdot 70 = \boxed{53}
\end{aligned}$$

Todo lo que queda por hacer es tomar el máximo sobre estas utilidades calculadas para determinar la MEU:

$$MEU(F = \text{malo}) = \max_aEU(a \mid \text{malo}) = \boxed{53}$$

La acción que produce la utilidad máxima esperada es *tomar*, y por lo tanto esta es la acción que nos recomienda la red de decisión. Más formalmente, la acción que produce la MEU se puede determinar tomando el **argmax** sobre las utilidades esperadas.

## 7.2.1 Árboles de resultados

Mencionamos al comienzo de esta nota que las redes de decisión involucraban algunos elementos al estilo expectimax, así que analicemos qué significa exactamente eso. Podemos desentrañar la selección de una acción correspondiente a la que maximiza la utilidad esperada en una red de decisión como un **árbol de resultados**. Nuestro ejemplo de pronóstico del tiempo anterior se desenreda en el siguiente árbol de resultados:

<img src="{{ site.baseurl }}/assets/images/outcome-tree.png" alt="Árbol de resultados" />

El nodo raíz en la parte superior es un nodo maximizador, al igual que en expectimax, y está controlado por nosotros. Seleccionamos una acción, que nos lleva al siguiente nivel en el árbol, controlado por nodos de azar. En este nivel, los nodos de azar se resuelven en diferentes nodos de utilidad en el nivel final con probabilidades correspondientes a las probabilidades posteriores derivadas de la inferencia probabilística ejecutada en la red bayesiana subyacente. ¿Qué hace exactamente que esto sea diferente del expectimax vainilla? La única diferencia real es que para los árboles de resultados anotamos nuestros nodos con lo que sabemos en cualquier momento dado (dentro de las llaves).
