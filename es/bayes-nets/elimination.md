---
title: '6.6 Inferencia exacta en redes bayesianas'
parent: 6. Redes Bayesianas
nav_order: 6
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.6 Inferencia exacta en redes bayesianas

<p>
</p>
La inferencia es el problema de encontrar el valor de alguna distribución de probabilidad $$P(Q_1 \dots Q_k | e_1 \dots e_k)$$, como se detalla en la sección Inferencia probabilística al comienzo de la nota. Dada una red bayesiana, podemos resolver este problema ingenuamente formando la PDF conjunta y utilizando la inferencia por enumeración. Esto requiere la creación e iteración sobre una tabla exponencialmente grande.

## 6.6.1 Eliminación de variables

Un enfoque alternativo es eliminar las variables ocultas una por una. Para **eliminar** una variable $$X$$, nosotros:

1. Unimos (multiplicamos juntos) todos los factores que involucran a $$X$$.
2. Sumamos $$X$$.

Un **factor** se define simplemente como una _probabilidad no normalizada_. En todos los puntos durante la eliminación de variables, cada factor será proporcional a la probabilidad a la que corresponde, pero la distribución subyacente para cada factor no necesariamente sumará 1 como debería hacerlo una distribución de probabilidad. El pseudocódigo para la eliminación de variables está aquí:

<img src="{{ site.baseurl }}/assets/images/VarElim.png" alt="Eliminación de variables" />

Hagamos estas ideas más concretas con un ejemplo. Supongamos que tenemos un modelo como se muestra a continuación, donde $$T$$, $$C$$, $$S$$ y $$E$$ pueden tomar valores binarios. Aquí, $$T$$ representa la probabilidad de que un aventurero tome un tesoro, $$C$$ representa la probabilidad de que una jaula caiga sobre el aventurero dado que toma el tesoro, $$S$$ representa la probabilidad de que se liberen serpientes si un aventurero toma el tesoro, y $$E$$ representa la probabilidad de que el aventurero escape dada la información sobre el estado de la jaula y las serpientes.

<img src="{{ site.baseurl }}/assets/images/another_bayes_nets.png" alt="Eliminación de variables" />

<p>
</p>
En este caso, tenemos los factores $$P(T)$$, $$P(C|T)$$, $$P(S|T)$$ y $$P(E|C,S)$$. Supongamos que queremos calcular $$P(T | +e)$$. El enfoque de inferencia por enumeración sería formar la PDF conjunta de 16 filas $$P(T, C, S, E)$$, seleccionar solo las filas correspondientes a +e, luego sumar $$C$$ y $$S$$ y finalmente normalizar.
<p>
</p>
El enfoque alternativo es eliminar $$C$$, luego $$S$$, una variable a la vez. Procederíamos de la siguiente manera:

<p>
</p>
Unir (multiplicar) todos los factores que involucran a $$C$$, formando $$f_1(C, +e, T, S) = P(C|T) \cdot P(+e|C, S)$$. A veces esto se escribe como $$P(C, +e | T, S)$$.
<p>
</p>
Sumar $$C$$ de este nuevo factor, dejándonos con un nuevo factor $$f_2(+e, T, S)$$, a veces escrito como $$P(+e | T, S)$$.
<p>
</p>
Unir todos los factores que involucran a $$S$$, formando $$F_3(+e, S, T)=P(S|T) \cdot f_2(+e, T, S)$$, a veces escrito como $$P(+e, S | T)$$.
<p>
</p>
Sumar $$S$$, produciendo $$f_4(+e, T)$$, a veces escrito como $$P(+e | T)$$.
<p>
</p>
Unir los factores restantes, lo que da $$f_5(+e, T) = f_4(+e, T) \cdot P(T)$$.
<p>
</p>
Una vez que tenemos $$f_5(+e, T)$$, podemos calcular fácilmente $$P(T|+e)$$ normalizando.

Al escribir un factor que resulta de una unión, podemos usar la notación de factor como $$f_1(C, +e, T, S)$$, que ignora la barra de condicionamiento y simplemente proporciona una lista de variables que se incluyen en este factor.

<p>
</p>
Alternativamente, podemos escribir $$P(C, +e | T, S)$$, incluso si no se garantiza que sea una distribución de probabilidad válida (por ejemplo, es posible que las filas no sumen 1). Para derivar esta expresión mecánicamente, tenga en cuenta que todas las variables en el lado izquierdo de las barras de condicionamiento en los factores originales (aquí, $$C$$ en $$P(C|T)$$ y $$E$$ en $$P(E|C,S)$$) permanecen en el lado izquierdo de la barra. Luego, todas las variables restantes (aquí, $$T$$ y $$S$$) van al lado derecho de la barra.

Este enfoque para escribir factores se basa en aplicaciones repetidas de la regla de la cadena. En el ejemplo anterior, sabemos que no podemos tener una variable en ambos lados de la barra condicional. Además, sabemos:

$$P(T, C, S, +e) = P(T) P(S | T)  P(C | T) P(+e | C, S) = P(S, T) P(C | T) P(+e | C, S)$$

y entonces:

$$P(C | T) P(+e | C, S) = \frac{P(T, C, S, +e)}{P(S, T)} = P(C, +e | T, S)$$

Si bien el proceso de eliminación de variables es más complejo conceptualmente, el tamaño máximo de cualquier factor generado es de solo 8 filas en lugar de 16, como sería si formáramos la PDF conjunta completa.

<p>
</p>
Una forma alternativa de ver el problema es observar que el cálculo de $$P(T|+e)$$ se puede hacer a través de la inferencia por enumeración de la siguiente manera:

$$\alpha \sum_s{\sum_c{P(T)P(s|T)P(c|T)P(+e|c,s)}}$$

o por eliminación de variables de la siguiente manera:

$$\alpha P(T)\sum_s{P(s|T)\sum_c{P(c|T)P(+e|c,s)}}$$

Podemos ver que las ecuaciones son equivalentes, ¡excepto que en la eliminación de variables hemos movido términos que son irrelevantes para las sumas fuera de cada suma!

Como nota final sobre la eliminación de variables, es importante observar que solo mejora la inferencia por enumeración si podemos limitar el tamaño del factor más grande a un valor razonable.
