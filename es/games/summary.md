---
title: 3.6 Resumen
parent: 3. Juegos
nav_order: 6
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 3.6 Resumen

Cambiamos de marcha de considerar problemas de búsqueda estándar donde simplemente intentamos encontrar un camino desde nuestro punto de partida hasta algún objetivo, a considerar problemas de búsqueda adversaria donde podemos tener oponentes que intentan impedirnos alcanzar nuestro objetivo. Se consideraron dos algoritmos principales:

- **Minimax** - Se usa cuando nuestro(s) oponente(s) se comporta(n) de manera óptima, y se puede optimizar usando poda $$\alpha$$-$$\beta$$. Minimax proporciona acciones más conservadoras que expectimax, por lo que tiende a producir resultados favorables cuando el oponente también es desconocido.
- **Expectimax** - Se usa cuando nos enfrentamos a oponente(s) subóptimo(s), usando una distribución de probabilidad sobre los movimientos que creemos que harán para calcular el valor esperado de los estados.

En la mayoría de los casos, es demasiado costoso computacionalmente ejecutar los algoritmos anteriores hasta el nivel de los nodos terminales en el árbol de juego bajo consideración, por lo que introdujimos la noción de funciones de evaluación para la terminación temprana. Para problemas con grandes factores de ramificación, describimos los algoritmos MCTS y UCT. Tales algoritmos son fácilmente paralelizables, lo que permite que se lleve a cabo una gran cantidad de despliegues utilizando hardware moderno.

Finalmente, consideramos el problema de los juegos generales, donde las reglas no son necesariamente de suma cero.
