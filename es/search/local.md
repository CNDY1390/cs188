---
title: 1.5 Búsqueda local
parent: 1. Búsqueda
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 1.5 Búsqueda local

En la nota anterior, queríamos encontrar el estado objetivo, junto con el camino óptimo para llegar allí. Pero en algunos problemas, solo nos importa encontrar el estado objetivo: reconstruir el camino puede ser trivial. Por ejemplo, en Sudoku, la configuración óptima es el objetivo. Una vez que lo sabes, sabes cómo llegar allí llenando los cuadrados uno por uno.

Los algoritmos de búsqueda local nos permiten encontrar estados objetivo sin preocuparnos por el camino para llegar allí. En los problemas de búsqueda local, el espacio de estados consiste en conjuntos de soluciones "completas". Usamos estos algoritmos para tratar de encontrar una configuración que satisfaga algunas restricciones u optimice alguna función objetivo.

<img src="{{ site.baseurl }}/assets/images/maxima_global_local.png" alt="Gráfico de función objetivo" />

La figura anterior muestra el gráfico unidimensional de una función objetivo en el espacio de estados. Para esa función, deseamos encontrar el estado que corresponde al valor objetivo más alto. La idea básica de los algoritmos de búsqueda local es que, desde cada estado, se mueven localmente hacia estados que tienen un valor objetivo más alto hasta que se alcanza un máximo (con suerte, el global). Cubriremos cuatro de estos algoritmos: **ascensión de colinas (hill-climbing)**, **recocido simulado (simulated annealing)**, **búsqueda de haz local (local beam search)** y **algoritmos genéticos**. Todos estos algoritmos también se utilizan en tareas de optimización para maximizar o minimizar una función objetivo.

## 1.5.1 Búsqueda de ascensión de colinas (Hill-Climbing Search)

El algoritmo de búsqueda de ascensión de colinas (o **ascenso más pronunciado**) se mueve desde el estado actual hacia el estado vecino que aumenta más el valor objetivo. El algoritmo no mantiene un árbol de búsqueda, sino que solo rastrea los estados y los valores correspondientes del objetivo. La "codicia" de la ascensión de colinas la hace vulnerable a quedar atrapada en **máximos locales** (ver figura 4.1), ya que localmente esos puntos aparecen como máximos globales para el algoritmo, y **mesetas** (ver figura 4.1). Las mesetas se pueden clasificar en áreas "planas" en las que ninguna dirección conduce a una mejora ("máximos locales planos") o áreas planas desde las cuales el progreso puede ser lento ("hombros").

Se han propuesto variantes de la ascensión de colinas, como la **ascensión de colinas estocástica** (que selecciona una acción aleatoriamente entre los posibles movimientos ascendentes). Se ha demostrado en la práctica que la ascensión de colinas estocástica converge a máximos más altos a costa de más iteraciones. Otra variante, **movimientos laterales aleatorios**, permite movimientos que no aumentan estrictamente el objetivo, ayudando al algoritmo a escapar de los "hombros".

<img src="{{ site.baseurl }}/assets/images/hill_climb-1.png" alt="Algoritmo de ascensión de colinas" />

El pseudocódigo de la ascensión de colinas se puede ver arriba. Como sugiere el nombre, el algoritmo se mueve iterativamente a un estado con un valor objetivo más alto hasta que no es posible tal progreso. La ascensión de colinas es incompleta. La **ascensión de colinas con reinicio aleatorio**, por otro lado, realiza una serie de búsquedas de ascensión de colinas desde estados iniciales elegidos aleatoriamente, lo que la hace trivialmente completa ya que, en algún momento, un estado inicial elegido aleatoriamente puede converger al máximo global.

Como nota, más adelante en este curso, encontrará el término "descenso de gradiente". Es exactamente la misma idea que la ascensión de colinas, excepto que en lugar de maximizar una función objetivo, querremos minimizar una función de costo.

## 1.5.2 Búsqueda de recocido simulado (Simulated Annealing Search)

El segundo algoritmo de búsqueda local que cubriremos es el recocido simulado. El recocido simulado tiene como objetivo combinar el paseo aleatorio (moverse aleatoriamente a estados cercanos) y la ascensión de colinas para obtener un algoritmo de búsqueda completo y eficiente. En el recocido simulado, permitimos movimientos a estados que pueden disminuir el objetivo.

El algoritmo elige un movimiento aleatorio en cada paso de tiempo. Si el movimiento conduce a un valor objetivo más alto, siempre se acepta. Si conduce a un valor objetivo más pequeño, el movimiento se acepta con cierta probabilidad. Esta probabilidad está determinada por el parámetro de temperatura, que inicialmente es alto (permitiendo más movimientos "malos") y disminuye de acuerdo con algún "programa". Teóricamente, si la temperatura se reduce lo suficientemente lento, el algoritmo de recocido simulado alcanzará el máximo global con una probabilidad cercana a 1.

<img src="{{ site.baseurl }}/assets/images/sim_ann-1.png" alt="Algoritmo de recocido simulado" />

## 1.5.3 Búsqueda de haz local (Local Beam Search)

La búsqueda de haz local es otra variante del algoritmo de búsqueda de ascensión de colinas. La diferencia clave entre los dos es que la búsqueda de haz local realiza un seguimiento de $$k$$ estados (hilos) en cada iteración. El algoritmo comienza con una inicialización aleatoria de $$k$$ estados, y en cada iteración, selecciona $$k$$ nuevos estados, como se hace en la ascensión de colinas. Estos no son solo $$k$$ copias del algoritmo de ascensión de colinas regular. Crucialmente, el algoritmo selecciona los $$k$$ mejores estados sucesores de la lista completa de estados sucesores de todos los hilos. Si alguno de los hilos encuentra el valor óptimo, el algoritmo se detiene.

Los $$k$$ hilos pueden compartir información entre ellos, permitiendo que los hilos "buenos" (para los cuales los objetivos son altos) "atraigan" a los otros hilos a esa región también.

La búsqueda de haz local también es susceptible a atascarse en regiones "planas" como lo hace la ascensión de colinas. La **búsqueda de haz estocástica**, análoga a la ascensión de colinas estocástica, puede aliviar este problema.

## 1.5.4 Algoritmos genéticos

Finalmente, presentamos los **algoritmos genéticos**, que son una variante de la búsqueda de haz local y se utilizan ampliamente en muchas tareas de optimización. Como indica el nombre, los algoritmos genéticos se inspiran en la evolución. Los algoritmos genéticos comienzan como una búsqueda de haz con $$k$$ estados inicializados aleatoriamente llamados **población**. Los estados (llamados **individuos**) se representan como una cadena sobre un alfabeto finito.

Revisemos el **problema de las $$8$$-Reinas** presentado en la clase. Como resumen, las $$8$$-Reinas es un problema de satisfacción de restricciones donde nuestro objetivo es situar $$8$$ reinas en un tablero de $$8$$-por-$$8$$. La solución que satisface las restricciones no tendrá ningún **par de reinas atacantes**, que son reinas en la misma fila, columna o diagonal. Todos los algoritmos cubiertos anteriormente se pueden utilizar para abordar el problema de las $$8$$-Reinas.

<img src="{{ site.baseurl }}/assets/images/8queen-1.jpg" alt="Problema de las 8-Reinas" />

Para un algoritmo genético, representamos cada una de las ocho reinas con un número del $$1-8$$, que representa la ubicación de cada reina en su columna (columna (a) en la Fig. 4.6). Cada individuo se evalúa utilizando una función de evaluación (**función de aptitud**), y se clasifican de acuerdo con los valores de esa función. Para el problema de las $$8$$-Reinas, este es el número de pares de reinas no atacantes (sin conflicto).

<img src="{{ site.baseurl }}/assets/images/gen2-1.png" alt="Ejemplo de algoritmo genético" />

La probabilidad de elegir un estado para "reproducirse" es proporcional al valor de ese estado. Procedemos a seleccionar pares de estados para reproducirse muestreando de estas probabilidades (columna (c) en la Fig. 4.6). La descendencia se genera cruzando las cadenas de los padres en el punto de cruce. El punto de cruce se elige aleatoriamente para cada par. Finalmente, cada descendiente es susceptible a alguna mutación aleatoria con probabilidad independiente. El pseudocódigo para el algoritmo genético se puede ver a continuación.

<img src="{{ site.baseurl }}/assets/images/genetic-1.png" alt="Pseudocódigo de algoritmo genético" />

Similar a la búsqueda de haz estocástica, los algoritmos genéticos intentan moverse cuesta arriba mientras exploran el espacio de estados e intercambian información entre hilos. Su principal ventaja es el uso de cruces: grandes bloques de letras que han evolucionado y conducen a altas valoraciones se pueden combinar con otros bloques similares para producir una solución con una puntuación total alta.

<img src="{{ site.baseurl }}/assets/images/8queen-2.jpg" alt="Solución de 8-Reinas" />
