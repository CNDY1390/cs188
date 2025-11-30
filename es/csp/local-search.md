---
title: 2.5 Búsqueda local
parent: 2. CSPs
nav_order: 5
layout: page
lang: es
header-includes:
    \pagenumbering{gobble}
---

# 2.5 Búsqueda local

Como último tema de interés, la búsqueda con retroceso no es el único algoritmo que existe para resolver problemas de satisfacción de restricciones. Otro algoritmo ampliamente utilizado es la **búsqueda local**, para la cual la idea es infantilmente simple pero notablemente útil. La búsqueda local funciona mediante una mejora iterativa: comience con alguna asignación aleatoria de valores, luego seleccione iterativamente una variable en conflicto aleatoria y reasigne su valor al que viola la menor cantidad de restricciones hasta que no existan más violaciones de restricciones (una política conocida como **heurística de conflictos mínimos**). Bajo tal política, los problemas de satisfacción de restricciones como $$N$$-reinas se vuelven muy eficientes en tiempo y espacio para resolver. Por ejemplo, en el siguiente ejemplo con 4 reinas, llegamos a una solución después de solo 2 iteraciones:

<img src="{{ site.baseurl }}/assets/images/four-queens.png" alt="Cuatro reinas" />

De hecho, la búsqueda local parece ejecutarse en un tiempo casi constante y tener una alta probabilidad de éxito no solo para $$N$$-reinas con $$N$$ arbitrariamente grande, ¡sino también para cualquier CSP generado aleatoriamente! Sin embargo, a pesar de estas ventajas, la búsqueda local es incompleta y subóptima, por lo que no necesariamente convergerá a una solución óptima. Además, hay una proporción crítica alrededor de la cual el uso de la búsqueda local se vuelve extremadamente costoso:

<img src="{{ site.baseurl }}/assets/images/critical-ratio.png" alt="Proporción crítica" />

La figura anterior muestra el gráfico unidimensional de una función objetivo en el espacio de estados. Para esa función, deseamos encontrar el estado que corresponde al valor objetivo más alto. La idea básica de los algoritmos de búsqueda local es que, desde cada estado, se mueven localmente hacia estados que tienen un valor objetivo más alto hasta que se alcanza un máximo (con suerte, el global).

<img src="{{ site.baseurl }}/assets/images/maxima_global_local.png" alt="Máximos globales y locales" />

Cubriremos tres de estos algoritmos: **ascensión de colinas**, **recocido simulado** y **algoritmos genéticos**. Todos estos algoritmos también se utilizan en tareas de optimización para maximizar o minimizar una función objetivo.

---

## 2.5.1 Búsqueda de ascensión de colinas (Hill-Climbing Search)

El algoritmo de búsqueda de ascensión de colinas (o **ascenso más pronunciado**) se mueve desde el estado actual hacia un estado vecino que aumenta el valor objetivo. El algoritmo no mantiene un árbol de búsqueda, sino solo los estados y los valores correspondientes del objetivo. La "codicia" de la ascensión de colinas la hace vulnerable a quedar atrapada en **máximos locales** (ver figura a continuación), ya que localmente esos puntos aparecen como máximos globales para el algoritmo, y **mesetas**. Las mesetas se pueden clasificar en áreas "planas" en las que ninguna dirección conduce a una mejora ("máximos locales planos") o áreas planas desde las cuales el progreso puede ser lento ("hombros"). Se han propuesto variantes de la ascensión de colinas, como la **ascensión de colinas estocástica**, que selecciona una acción aleatoriamente entre los movimientos ascendentes. Se ha demostrado en la práctica que esta versión de la ascensión de colinas converge a máximos más altos a costa de más iteraciones.

<img src="{{ site.baseurl }}/assets/images/hill_climb-1.png" alt="Ascensión de colinas" />

El pseudocódigo de la ascensión de colinas se puede ver arriba. Como sugiere el nombre, el algoritmo se mueve iterativamente a un estado con un valor objetivo más alto hasta que no es posible tal progreso. La ascensión de colinas es incompleta. La **ascensión de colinas con reinicio aleatorio**, por otro lado, realiza una serie de búsquedas de ascensión de colinas, cada vez desde un estado inicial elegido aleatoriamente, y es trivialmente completa, ya que en algún momento el estado inicial elegido aleatoriamente coincidirá con el máximo global.

---

## 2.5.2 Búsqueda de recocido simulado (Simulated Annealing Search)

El segundo algoritmo de búsqueda local que cubriremos es el recocido simulado. El recocido simulado tiene como objetivo combinar el paseo aleatorio (moverse aleatoriamente a estados cercanos) y la ascensión de colinas para obtener un algoritmo de búsqueda completo y eficiente. En el recocido simulado, permitimos movimientos a estados que pueden disminuir el objetivo. Más específicamente, el algoritmo en cada estado elige un movimiento aleatorio. Si el movimiento conduce a un objetivo más alto, siempre se acepta. Si, por otro lado, conduce a objetivos más pequeños, entonces el movimiento se acepta con cierta probabilidad. Esta probabilidad está determinada por el parámetro de temperatura, que inicialmente es alto (se permiten más movimientos "malos") y disminuye de acuerdo con algún programa. Si la temperatura se reduce lo suficientemente lento, entonces el algoritmo de recocido simulado alcanzará el máximo global con una probabilidad cercana a 1.

<img src="{{ site.baseurl }}/assets/images/sim_ann-1.png" alt="Recocido simulado" />

---

## 2.5.3 Algoritmos genéticos

Finalmente, presentamos los **algoritmos genéticos**, que son una variante de la búsqueda de haz local y también se utilizan ampliamente en muchas tareas de optimización. Los algoritmos genéticos comienzan como una búsqueda de haz con $$k$$ estados inicializados aleatoriamente llamados **población**. Los estados (o **individuos**) se representan como una cadena sobre un alfabeto finito. Para comprender mejor el tema, revisemos el problema de las 8-Reinas presentado en clase. Para el problema de las 8-Reinas, podemos representar cada uno de los ocho individuos con un número que varía de 1 a 8, que representa la ubicación de cada reina en la columna (columna (a) en la Fig. 4.6). Cada individuo se evalúa utilizando una función de evaluación (**función de aptitud**), y se clasifican de acuerdo con los valores de esa función. Para el problema de las 8-Reinas, este es el número de pares de reinas que no se atacan.

<img src="{{ site.baseurl }}/assets/images/gen2-1.png" alt="Ejemplo de algoritmo genético" />

La probabilidad de elegir un estado para "reproducirse" es proporcional al valor de ese estado. Procedemos a seleccionar pares de estados para reproducirse de acuerdo con estas probabilidades (columna (c) en la Fig. 4.6). La descendencia se genera cruzando las cadenas de los padres en el punto de cruce. Ese punto de cruce se elige aleatoriamente para cada par. Finalmente, cada descendiente es susceptible a alguna mutación aleatoria con probabilidad independiente. El pseudocódigo del algoritmo genético se puede ver en la siguiente imagen.

<img src="{{ site.baseurl }}/assets/images/genetic-1.png" alt="Pseudocódigo de algoritmo genético" />

Los algoritmos genéticos intentan moverse cuesta arriba mientras exploran el espacio de estados e intercambian información entre hilos. Su principal ventaja es el uso de cruces, ya que esto permite que grandes bloques de letras, que han evolucionado y conducen a altas valoraciones, se combinen con otros bloques similares y produzcan una solución con una puntuación total alta.
