---
title: 8.5 Resumen
parent: 8. HMMs
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 8.5 Resumen

Vimos que los **modelos de Markov** pueden considerarse como redes bayesianas en forma de cadena y de longitud infinita. También aprendimos que los modelos de Markov obedecen la **propiedad de Markov**, lo que significa que la distribución de la cantidad que estamos modelando depende solo del valor de la cantidad en el paso de tiempo anterior. También aprendimos que podemos calcular la distribución de la cantidad modelada en cualquier paso de tiempo dado utilizando una técnica llamada **algoritmo mini-forward**, y que si dejamos que el tiempo vaya al infinito, esta distribución eventualmente convergerá a la **distribución estacionaria**.

También cubrimos dos nuevos tipos de modelos:
- *Modelos de Markov*, que codifican variables aleatorias dependientes del tiempo que poseen la propiedad de Markov. Podemos calcular una distribución de creencias en cualquier paso de tiempo de nuestra elección para un modelo de Markov utilizando inferencia probabilística con el algoritmo mini-forward.
- *Modelos Ocultos de Markov*, que son modelos de Markov con la propiedad adicional de que se puede observar nueva evidencia que puede afectar nuestra distribución de creencias en cada paso de tiempo. Para calcular la distribución de creencias en cualquier paso de tiempo dado con Modelos Ocultos de Markov, usamos el algoritmo forward.

A veces, ejecutar una inferencia exacta en estos modelos puede ser demasiado costoso computacionalmente, en cuyo caso podemos usar el filtrado de partículas como un método de inferencia aproximada.
