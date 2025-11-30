---
title: 4.5 Resumen
parent: 4. MDPs
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 4.5 Resumen

El material presentado anteriormente tiene muchas oportunidades de confusión. Cubrimos la iteración de valor, la iteración de políticas, la extracción de políticas y la evaluación de políticas, todas las cuales se ven similares, utilizando la ecuación de Bellman con una variación sutil.

A continuación se muestra un resumen del propósito de cada algoritmo:

- **Iteración de valor**: Se utiliza para calcular los valores óptimos de los estados, mediante actualizaciones iterativas hasta la convergencia.
- **Evaluación de políticas**: Se utiliza para calcular los valores de los estados bajo una política específica.
- **Extracción de políticas**: Se utiliza para determinar una política dada alguna función de valor de estado. Si los valores de estado son óptimos, esta política será óptima. Este método se utiliza después de ejecutar la iteración de valor para calcular una política óptima a partir de los valores de estado óptimos, o como una subrutina en la iteración de políticas para calcular la mejor política para los valores de estado estimados actualmente.
- **Iteración de políticas**: Una técnica que encapsula tanto la evaluación de políticas como la extracción de políticas y se utiliza para la convergencia iterativa a una política óptima. Tiende a superar a la iteración de valor en virtud del hecho de que las políticas generalmente convergen mucho más rápido que los valores de los estados.
