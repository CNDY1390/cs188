---
title: 1.6 Resumen
parent: 1. Búsqueda
nav_order: 6
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 1.5 Resumen

Discutimos los *problemas de búsqueda* y sus componentes: un *espacio de estados*, un conjunto de *acciones*, una *función de transición*, un *costo de acción*, un *estado inicial* y un *estado objetivo*. El agente interactúa con el entorno a través de sus sensores y sus actuadores. La función del agente describe lo que hace el agente en todas las circunstancias. La racionalidad del agente significa que el agente busca maximizar su utilidad esperada. Finalmente, definimos nuestros entornos de tareas utilizando descripciones PEAS.

Con respecto a los problemas de búsqueda, se pueden resolver utilizando una variedad de técnicas de búsqueda, incluidas, entre otras, las cinco que estudiamos en CS 188:

- *Búsqueda en anchura (Breadth-first Search)*
- *Búsqueda en profundidad (Depth-first Search)*
- *Búsqueda de costo uniforme (Uniform Cost Search)*
- *Búsqueda voraz (Greedy Search)*
- *Búsqueda A* (A* Search)*

Las primeras tres técnicas de búsqueda enumeradas anteriormente son ejemplos de *búsqueda no informada*, mientras que las dos últimas son ejemplos de *búsqueda informada* que utilizan *heurísticas* para estimar la distancia al objetivo y optimizar el rendimiento.

Además, hicimos una distinción entre los algoritmos de *búsqueda en árbol* y *búsqueda en grafo* para las técnicas anteriores.

También discutimos los *algoritmos de búsqueda local* y su motivación. Podemos utilizar estos enfoques cuando no nos importa el camino hacia algún estado objetivo y queremos satisfacer restricciones u optimizar un objetivo. ¡Los enfoques de búsqueda local nos permiten ahorrar espacio y encontrar soluciones adecuadas cuando trabajamos en grandes espacios de estados!

Repasamos algunos enfoques fundamentales de búsqueda local, que se basan unos en otros:

- *Ascensión de colinas (Hill-Climbing)*
- *Recocido simulado (Simulated Annealing)*
- *Búsqueda de haz local (Local Beam Search)*
- *Algoritmos genéticos (Genetic Algorithms)*

La idea de optimizar una función reaparecerá más adelante en este curso, especialmente cuando cubramos las redes neuronales.
