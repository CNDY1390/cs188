---
title: '6.3 Representación de redes bayesianas'
parent: 6. Redes Bayesianas
nav_order: 3
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.3 Representación de redes bayesianas

Si bien la inferencia por enumeración puede calcular probabilidades para cualquier consulta que podamos desear, representar una distribución conjunta completa en la memoria de una computadora no es práctico para problemas reales: si cada una de las $$n$$ variables que deseamos representar puede tomar $$d$$ valores posibles (tiene un **dominio** de tamaño $$d$$), entonces nuestra tabla de distribución conjunta tendrá $$d^n$$ entradas, ¡exponencial en el número de variables y bastante poco práctico de almacenar!

Las redes bayesianas evitan este problema aprovechando la idea de probabilidad condicional. En lugar de almacenar información en una tabla gigante, las probabilidades se distribuyen a través de una serie de tablas de probabilidad condicional más pequeñas junto con un **grafo acíclico dirigido** (DAG) que captura las relaciones entre las variables. Las tablas de probabilidad locales y el DAG juntos codifican suficiente información para calcular cualquier distribución de probabilidad que podríamos haber calculado dada la gran distribución conjunta completa. Veremos cómo funciona esto en la siguiente sección.

Definimos formalmente una red bayesiana como compuesta por:

<p></p>
1.Un grafo acíclico dirigido de nodos, uno por variable $$X$$.
<p></p>
2.Una distribución condicional para cada nodo $$P(X | A1 ... An)$$, donde $$Ai$$ es el $$i$$-ésimo padre de $$X$$, almacenada como una **tabla de probabilidad condicional** o CPT. Cada CPT tiene $$n+2$$ columnas: una para los valores de cada una de las $$n$$ variables padre $$A1 ... An$$, una para los valores de $$X$$, y una para la probabilidad condicional de $$X$$ dado sus padres.
<p></p>

La estructura del grafo de la red bayesiana codifica las relaciones de independencia condicional entre diferentes nodos. Estas independencias condicionales nos permiten almacenar múltiples tablas pequeñas en lugar de una grande.

Es importante recordar que los bordes entre los nodos de la red bayesiana no significan que haya específicamente una relación _causal_ entre esos nodos, o que las variables sean necesariamente dependientes entre sí. Simplemente significa que puede haber _alguna_ relación entre los nodos.

Como ejemplo de una red bayesiana, considere un modelo donde tenemos cinco variables aleatorias binarias descritas a continuación:

- **B**: Ocurre un robo.
- **A**: Suena la alarma.
- **E**: Ocurre un terremoto.
- **J**: John llama.
- **M**: Mary llama.

Supongamos que la alarma puede sonar si ocurre un robo o un terremoto, y que Mary y John llamarán si escuchan la alarma. Podemos representar estas dependencias con el grafo que se muestra a continuación.

<img src="{{ site.baseurl }}/assets/images/basic_bayes_nets.png" alt="Ejemplo básico de redes bayesianas" />

<p></p>
En esta red bayesiana, almacenaríamos las tablas de probabilidad $$P(B)$$, $$P(E)$$, $$P(A | B, E)$$, $$P(J | A)$$ y $$P(M | A)$$.

Dadas todas las CPT para un grafo, podemos calcular la probabilidad de una asignación dada usando la siguiente regla:

$$P(X1, X2, ..., Xn) = \prod_{i=1}^n{P(X_i | parents(X_i))}$$

Para el modelo de alarma anterior, en realidad podemos calcular la probabilidad de una probabilidad conjunta de la siguiente manera:

$$P(-b, -e, +a, +j, -m) = P(-b) \cdot P(-e) \cdot P(+a | -b, -e) \cdot P(+j | +a) \cdot P(-m | +a)$$

Veremos cómo se mantiene esta relación en la siguiente sección.

Como verificación de la realidad, es importante internalizar que las redes bayesianas son solo un tipo de modelo. Los modelos intentan capturar la forma en que funciona el mundo, pero debido a que siempre son una simplificación, siempre están equivocados. Sin embargo, con buenas opciones de modelado, aún pueden ser aproximaciones lo suficientemente buenas como para ser útiles para resolver problemas reales en el mundo real.

En general, un buen modelo puede no tener en cuenta todas las variables o incluso todas las interacciones entre variables. Pero al hacer suposiciones de modelado en la estructura del grafo, podemos producir técnicas de inferencia increíblemente eficientes que a menudo son más útiles en la práctica que procedimientos simples como la inferencia por enumeración.
