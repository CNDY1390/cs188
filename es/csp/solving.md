---
title: 2.2 Resolver Problemas de Satisfacción de Restricciones
parent: 2. CSPs
nav_order: 2
layout: page
lang: es
header-includes:
    \pagenumbering{gobble}
---

# 2.2 Resolver Problemas de Satisfacción de Restricciones

Los problemas de satisfacción de restricciones se resuelven tradicionalmente utilizando un algoritmo de búsqueda conocido como **búsqueda con retroceso (backtracking search)**. La búsqueda con retroceso es una optimización de la búsqueda en profundidad utilizada específicamente para el problema de la satisfacción de restricciones, con mejoras provenientes de dos principios principales:

1. Fijar un orden para las variables y seleccionar valores para las variables en este orden. Debido a que las asignaciones son conmutativas (por ejemplo, asignar $$WA = \text{Rojo},\:\: NT = \text{Verde}$$ es idéntico a $$NT = \text{Verde},\:\: WA = \text{Rojo}$$), esto es válido.
2. Al seleccionar valores para una variable, seleccione solo valores que no entren en conflicto con ningún valor asignado previamente. Si no existen tales valores, retroceda y regrese a la variable anterior, cambiando su valor.

El pseudocódigo de cómo funciona el retroceso recursivo se presenta a continuación:

<img src="{{ site.baseurl }}/assets/images/backtracking-search-pseudo.png" alt="Pseudocódigo de búsqueda con retroceso" />

Para visualizar cómo funciona este proceso, considere los árboles de búsqueda parciales tanto para la búsqueda en profundidad como para la búsqueda con retroceso en el coloreado de mapas:

<img src="{{ site.baseurl }}/assets/images/dfs-vs-backtracking.png" alt="DFS vs Búsqueda con retroceso" />

Observe cómo DFS lamentablemente colorea todo de rojo antes de darse cuenta de la necesidad de cambio, e incluso entonces no avanza demasiado en la dirección correcta hacia una solución. Por otro lado, la búsqueda con retroceso solo asigna un valor a una variable si ese valor no viola ninguna restricción, lo que lleva a un retroceso significativamente menor. Aunque la búsqueda con retroceso es una gran mejora con respecto a la fuerza bruta de la búsqueda en profundidad, aún podemos obtener más ganancias en velocidad con mejoras adicionales a través del filtrado, el ordenamiento de variables/valores y la explotación estructural.
