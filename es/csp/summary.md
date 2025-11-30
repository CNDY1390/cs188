---
title: 2.6 Resumen
parent: 2. CSPs
nav_order: 6
layout: page
lang: es
header-includes:
    \pagenumbering{gobble}
---

# 2.6 Resumen

Es importante recordar que los problemas de satisfacción de restricciones en general no tienen un algoritmo eficiente que los resuelva en tiempo polinomial con respecto al número de variables involucradas. Sin embargo, al utilizar varias heurísticas, a menudo podemos encontrar soluciones en una cantidad de tiempo aceptable:

- *Filtrado* - El filtrado maneja la poda de los dominios de variables no asignadas con anticipación para evitar retrocesos innecesarios. Las dos técnicas de filtrado importantes que hemos cubierto son la *verificación hacia adelante* y la *consistencia de arco*.
- *Ordenamiento* - El ordenamiento maneja la selección de qué variable o valor asignar a continuación para que el retroceso sea lo menos probable posible. Para la selección de variables, aprendimos sobre una *política MRV* y para la selección de valores aprendimos sobre una *política LCV*.
- *Estructura* - Si un CSP está estructurado en árbol o cerca de estar estructurado en árbol, podemos ejecutar el algoritmo CSP estructurado en árbol en él para derivar una solución en tiempo lineal. De manera similar, si un CSP está cerca de estar estructurado en árbol, podemos usar el *condicionamiento de conjunto de corte* para transformar el CSP en uno o más CSP estructurados en árbol independientes y resolver cada uno de estos por separado.
