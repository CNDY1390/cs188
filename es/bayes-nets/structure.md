---
title: '6.4 Estructura de las redes bayesianas'
parent: 6. Redes Bayesianas
nav_order: 4
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.4 Estructura de las redes bayesianas

En esta clase, nos referiremos a dos reglas para las independencias de la red bayesiana que se pueden inferir al observar la estructura gráfica de la red bayesiana:

- **Cada nodo es condicionalmente independiente de todos sus nodos ancestros (no descendientes) en el grafo, dados todos sus padres.**

<img src="{{ site.baseurl }}/assets/images/parents.png" alt="Padres" />

- **Cada nodo es condicionalmente independiente de todas las demás variables dada su manta de Markov.** La manta de Markov de una variable consta de padres, hijos y otros padres de los hijos.

<img src="{{ site.baseurl }}/assets/images/blanket.png" alt="Manta de Markov" />

Usando estas herramientas, podemos volver a la afirmación de la sección anterior: que podemos obtener la distribución conjunta de todas las variables uniendo las CPT de la red bayesiana.

$$P(X_1, X_2, \dots, X_n) = \prod_{i=1}^n P(X_i | \text{padres}(X_i))$$

Esta relación entre la distribución conjunta y las CPT de la red bayesiana funciona debido a las relaciones de independencia condicional dadas por el grafo. Probaremos esto usando un ejemplo.

<p></p>
Revisemos el ejemplo anterior. Tenemos las CPT $$P(B)$$ , $$P(E)$$ , $$P(A |B,E)$$ , $$P(J | A)$$ y $$P(M | A)$$ , y el siguiente grafo:

<img src="{{ site.baseurl }}/assets/images/basic_bayes_nets.png" alt="Ejemplos básicos de redes bayesianas" />

Para esta red bayesiana, estamos tratando de probar la siguiente relación:

$$P(B, E, A, J, M) = P(B)P(E)P(A | B, E)P(J | A)P(M | A)$$

Podemos expandir la distribución conjunta de otra manera: usando la regla de la cadena. Si expandimos la distribución conjunta con ordenamiento topológico (padres antes que hijos), obtenemos la siguiente ecuación:

$$P(B, E, A, J, M) = P(B)P(E | B)P(A | B, E)P(J | B, E, A)P(M | B, E, A, J)$$

<p></p>
Observe que en la primera ecuación cada variable se representa en una CPT $$P(var | Padres(var))$$ , mientras que en la segunda ecuación, cada variable se representa en una CPT $$P(var | Padres(var), Ancestros(var))$$ .

Confiamos en la primera relación de independencia condicional anterior, que **cada nodo es condicionalmente independiente de todos sus nodos ancestros en el grafo, dados todos sus padres**[^1].

<p></p>
Por lo tanto, en una red bayesiana, $$P(var | Padres(var), Ancestros(var)) = P(var | Padres(var))$$ , por lo que las dos ecuaciones son iguales. Las independencias condicionales en una red bayesiana permiten que múltiples tablas de probabilidad condicional más pequeñas representen toda la distribución de probabilidad conjunta.

[^1]: En otros lugares, la suposición puede definirse como "un nodo es condicionalmente independiente de sus _no descendientes_ dados sus padres". Siempre queremos hacer la suposición mínima posible y probar lo que necesitamos, por lo que usaremos la suposición de ancestros.
