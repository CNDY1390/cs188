---
title: 2.1 Problemas de Satisfacción de Restricciones
parent: 2. CSPs
nav_order: 1
layout: page
lang: es
header-includes:
    \pagenumbering{gobble}
---

# 2.1 Problemas de Satisfacción de Restricciones

En la nota anterior, aprendimos cómo encontrar soluciones óptimas a problemas de búsqueda, un tipo de **problema de planificación**. Ahora, aprenderemos a resolver una clase de problemas relacionados, los **problemas de satisfacción de restricciones** (CSPs). A diferencia de los problemas de búsqueda, los CSP son un tipo de **problema de identificación**, problemas en los que simplemente debemos identificar si un estado es un estado objetivo o no, sin tener en cuenta cómo llegamos a ese objetivo. Los CSP se definen por tres factores:

1. *Variables* - Los CSP poseen un conjunto de $$N$$ variables $$X_1, \dots, X_N$$ que pueden tomar cada una un valor único de algún conjunto definido de valores.
2. *Dominio* - Un conjunto $$\{x_1, \dots, x_d\}$$ que representa todos los valores posibles que puede tomar una variable CSP.
3. *Restricciones* - Las restricciones definen restricciones sobre los valores de las variables, potencialmente con respecto a otras variables.

Considere el problema de identificación de $$N$$-reinas: dado un tablero de ajedrez de $$N \times N$$, ¿podemos encontrar una configuración en la que colocar $$N$$ reinas en el tablero de tal manera que no haya dos reinas que se ataquen entre sí?

<img src="{{ site.baseurl }}/assets/images/n-queens.png" alt="Problema de N-reinas" />

Podemos formular este problema como un CSP de la siguiente manera:

1. *Variables* - $$X_{ij}$$, con $$0 \leq i, j < N$$. Cada $$X_{ij}$$ representa una posición de cuadrícula en nuestro tablero de ajedrez de $$N \times N$$, con $$i$$ y $$j$$ especificando el número de fila y columna respectivamente.
2. *Dominio* - $$\{0, 1\}$$. Cada $$X_{ij}$$ puede tomar el valor 0 o 1, un valor booleano que representa la existencia de una reina en la posición $$(i, j)$$ en el tablero.
3. *Restricciones* - 
    - $$\forall i,j,k \:\: (X_{ij}, X_{ik}) \in \{(0, 0), (0, 1), (1, 0)\}$$. Esta restricción establece que si dos variables tienen el mismo valor para $$i$$, solo una de ellas puede tomar un valor de 1. Esto encapsula efectivamente la condición de que no puede haber dos reinas en la misma fila.
    - $$\forall i,j,k \:\: (X_{ij}, X_{kj}) \in \{(0, 0), (0, 1), (1, 0)\}$$. Casi idénticamente a la restricción anterior, esta restricción establece que si dos variables tienen el mismo valor para $$j$$, solo una de ellas puede tomar un valor de 1, encapsulando la condición de que no puede haber dos reinas en la misma columna.
    - $$\forall i,j,k \:\: (X_{ij}, X_{i+k,j+k}) \in \{(0, 0), (0, 1), (1, 0)\}$$. Con un razonamiento similar al anterior, podemos ver que esta restricción y la siguiente representan las condiciones de que no puede haber dos reinas en las mismas diagonales mayores o menores, respectivamente.
    - $$\forall i,j,k \:\: (X_{ij}, X_{i+k,j-k}) \in \{(0, 0), (0, 1), (1, 0)\}$$.
    - $$\sum_{i,j}X_{ij} = N$$. Esta restricción establece que debemos tener exactamente $$N$$ posiciones de cuadrícula marcadas con un 1, y todas las demás marcadas con un 0, capturando el requisito de que hay exactamente $$N$$ reinas en el tablero.

Los problemas de satisfacción de restricciones son **NP-duros**, lo que significa vagamente que no existe un algoritmo conocido para encontrar soluciones a ellos en tiempo polinomial. Dado un problema con $$N$$ variables con dominio de tamaño $$O(d)$$ para cada variable, hay $$O(d^N)$$ asignaciones posibles, exponenciales en el número de variables. A menudo podemos sortear esta advertencia formulando CSP como problemas de búsqueda, definiendo estados como **asignaciones parciales** (asignaciones de variables a CSP donde a algunas variables se les han asignado valores mientras que a otras no). Correspondientemente, la función sucesora para un estado CSP genera todos los estados con una nueva variable asignada, y la prueba de objetivo verifica que todas las variables estén asignadas y que todas las restricciones se satisfagan en el estado que está probando. Los problemas de satisfacción de restricciones tienden a tener una estructura significativamente mayor que los problemas de búsqueda tradicionales, y podemos explotar esta estructura combinando la formulación anterior con heurísticas apropiadas para afinar las soluciones en una cantidad de tiempo factible.

## 2.1.1 Grafos de restricciones

Presentemos un segundo ejemplo de CSP: coloreado de mapas. El coloreado de mapas resuelve el problema en el que se nos da un conjunto de colores y debemos colorear un mapa de tal manera que no haya dos estados o regiones adyacentes que tengan el mismo color.

<img src="{{ site.baseurl }}/assets/images/map-coloring-comic.png" alt="Cómic de coloreado de mapas" />

Los problemas de satisfacción de restricciones a menudo se representan como grafos de restricciones, donde los nodos representan variables y los bordes representan restricciones entre ellos. Hay muchos tipos diferentes de restricciones, y cada una se maneja de manera ligeramente diferente:

- *Restricciones unarias* - Las restricciones unarias involucran una sola variable en el CSP. No se representan en los grafos de restricciones, sino que simplemente se utilizan para podar el dominio de la variable que restringen cuando es necesario.
- *Restricciones binarias* - Las restricciones binarias involucran dos variables. Se representan en grafos de restricciones como bordes de grafos tradicionales.
- *Restricciones de orden superior* - Las restricciones que involucran tres o más variables también se pueden representar con bordes en un grafo CSP, simplemente se ven un poco poco convencionales.

Considere el coloreado del mapa de Australia:

<img src="{{ site.baseurl }}/assets/images/australia-map.png" alt="Mapa de Australia" />

Las restricciones en este problema son simplemente que no puede haber dos estados adyacentes del mismo color. Como resultado, al dibujar un borde entre cada par de estados que son adyacentes entre sí, podemos generar el grafo de restricciones para el coloreado del mapa de Australia de la siguiente manera:

<img src="{{ site.baseurl }}/assets/images/australia-graph.png" alt="Grafo de restricciones de Australia" />

El valor de los grafos de restricciones es que podemos usarlos para extraer información valiosa sobre la estructura de los CSP que estamos resolviendo. Al analizar el grafo de un CSP, podemos determinar cosas sobre él, como si está conectado/restringido de manera dispersa o densa y si tiene estructura de árbol o no. Cubriremos esto más a fondo a medida que discutamos la resolución de problemas de satisfacción de restricciones con más detalle.
