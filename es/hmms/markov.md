---
title: 8.1 Modelos de Markov
parent: 8. HMMs
nav_order: 1
layout: page
header-includes: \pagenumbering{gobble}
---

# 8.1 Modelos de Markov

En notas anteriores, hablamos sobre las redes bayesianas y cómo son una estructura maravillosa utilizada para representar de forma compacta las relaciones entre variables aleatorias. Ahora cubriremos una estructura intrínsecamente relacionada llamada **modelo de Markov**, que para los propósitos de este curso se puede considerar análoga a una red bayesiana similar a una cadena de longitud infinita. El ejemplo con el que trabajaremos en esta sección son las fluctuaciones diarias en los patrones climáticos. Nuestro modelo meteorológico será **dependiente del tiempo** (como lo son los modelos de Markov en general), lo que significa que tendremos una variable aleatoria separada para el clima en cada día. Si definimos $$W_i$$ como la variable aleatoria que representa el clima en el día $$i$$, el modelo de Markov para nuestro ejemplo meteorológico se vería así:

<img src="{{ site.baseurl }}/assets/images/weather-markov.png" alt="Modelo de Markov del clima" />
<p>
</p>
¿Qué información deberíamos almacenar sobre las variables aleatorias involucradas en nuestro modelo de Markov? Para rastrear cómo cambia nuestra cantidad bajo consideración (en este caso, el clima) con el tiempo, necesitamos conocer tanto su **distribución inicial** en el tiempo $$t = 0$$ como algún tipo de **modelo de transición** que caracterice la probabilidad de pasar de un estado a otro entre pasos de tiempo. La distribución inicial de un modelo de Markov se enumera mediante la tabla de probabilidad dada por $$P(W_0)$$ y el modelo de transición de la transición del estado $$i$$ al $$i + 1$$ viene dado por $$P(W_{i+1} | W_i)$$. Tenga en cuenta que este modelo de transición implica que el valor de $$W_{i+1}$$ es condicionalmente dependiente solo del valor de $$W_i$$. En otras palabras, el clima en el tiempo $$t = i + 1$$ satisface la **propiedad de Markov** o **propiedad sin memoria**, y es independiente del clima en todos los demás pasos de tiempo además de $$t = i$$.
<p>
</p>
Usando nuestro modelo de Markov para el clima, si quisiéramos reconstruir la conjunta entre $$W_0$$, $$W_1$$ y $$W_2$$ usando la regla de la cadena, querríamos:

$$P(W_0, W_1, W_2) = P(W_0)P(W_1 | W_0)P(W_2 | W_1, W_0)$$

<p>
</p>
Sin embargo, con nuestra suposición de que la propiedad de Markov es verdadera y $$W_0 \perp W_2 | W_1$$, la conjunta se simplifica a:
<p>
</p>

$$P(W_0, W_1, W_2) = P(W_0)P(W_1 | W_0)P(W_2 | W_1)$$

<p>
</p>
Y tenemos todo lo que necesitamos para calcular esto a partir del modelo de Markov. De manera más general, los modelos de Markov hacen la siguiente suposición de independencia en cada paso de tiempo: $$W_{i+1} \perp \{W_0, \dots, W_{i-1}\} | W_i$$. Esto nos permite reconstruir la distribución conjunta para las primeras $$n + 1$$ variables a través de la regla de la cadena de la siguiente manera:
<p>
</p>

$$P(W_0, W_1, \dots, W_n) = P(W_0)P(W_1|W_0)P(W_2|W_1)\dots P(W_n|W_{n-1}) = P(W_0)\prod_{i=0}^{n-1}P(W_{i+1}|W_{i})$$

<p>
</p>
Una suposición final que se hace típicamente en los modelos de Markov es que el modelo de transición es **estacionario**. En otras palabras, para todos los valores de $$i$$ (todos los pasos de tiempo), $$P(W_{i+1} | W_i)$$ es idéntico. Esto nos permite representar un modelo de Markov con solo dos tablas: una para $$P(W_0)$$ y otra para $$P(W_{i+1} | W_i)$$.
<p>
</p>
# 8.1.1 El algoritmo mini-forward

Ahora sabemos cómo calcular la distribución conjunta a través de los pasos de tiempo de un modelo de Markov. Sin embargo, esto no nos ayuda explícitamente a responder la pregunta de la distribución del clima en un día determinado $$t$$. Naturalmente, podemos calcular la conjunta y luego **marginar** (sumar) sobre todas las demás variables, pero esto suele ser extremadamente ineficiente ya que si tenemos $$j$$ variables, cada una de las cuales puede tomar $$d$$ valores, el tamaño de la distribución conjunta es $$O(d^j)$$. En cambio, presentaremos una técnica más eficiente llamada **algoritmo mini-forward**.

Así es como funciona. Por las propiedades de la marginación, sabemos que

$$P(W_{i+1}) = \sum_{w_i}P(w_i, W_{i+1})$$

Por la regla de la cadena, podemos volver a expresar esto de la siguiente manera:

<p>
</p>

$$\boxed{P(W_{i+1}) = \sum_{w_i}P(W_{i+1}|w_i)P(w_i)}$$

<p>
</p>
Esta ecuación debería tener algún sentido intuitivo: para calcular la distribución del clima en el paso de tiempo $$i + 1$$, miramos la distribución de probabilidad en el paso de tiempo $$i$$ dada por $$P(W_i)$$ y "avanzamos" este modelo un paso de tiempo con nuestro modelo de transición $$P(W_{i+1} | W_i)$$. Con esta ecuación, podemos calcular iterativamente la distribución del clima en cualquier paso de tiempo de nuestra elección comenzando con nuestra distribución inicial $$P(W_0)$$ y usándola para calcular $$P(W_1)$$, luego, a su vez, usando $$P(W_1)$$ para calcular $$P(W_2)$$, y así sucesivamente. Caminemos a través de un ejemplo, usando la siguiente distribución inicial y modelo de transición:

| $$ W_0 $$ | $$ P(W_0) $$ |
| :-------: | :----------: |
|    sol    |     0.8      |
|   lluvia    |     0.2      |

| $$ W_{i+1} $$ | $$ W_i $$ | $$ P(W_{i+1} \| W_i) $$ |
| :------------: | :-------: | :----------------------: |
|      sol       |    sol    |           0.6            |
|      lluvia      |    sol    |           0.4            |
|      sol       |   lluvia    |           0.1            |
|      lluvia      |   lluvia    |           0.9            |

Usando el algoritmo mini-forward, podemos calcular $$ P(W_1) $$ de la siguiente manera:

<p></p>
$$P(W_1 = sol) = \sum_{w_0}P(W_1 = sol | w_0)P(w_0)$$ $$= P(W_1 = sol | W_0 = sol)P(W_0 = sol) + P(W_1 = sol | W_0 = lluvia)P(W_0 = lluvia)$$ $$= 0.6 \cdot 0.8 + 0.1 \cdot 0.2 = \boxed{0.5}$$

<p></p>
$$P(W_1 = lluvia) = \sum_{w_0}P(W_1 = lluvia | w_0)P(w_0)$$ $$= P(W_1 = lluvia | W_0 = sol)P(W_0 = sol) + P(W_1 = lluvia | W_0 = lluvia)P(W_0 = lluvia)$$ $$= 0.4 \cdot 0.8 + 0.9 \cdot 0.2 = \boxed{0.5}$$

Por lo tanto, nuestra distribución para $$P(W_1)$$ es:

| **$$ W_1 $$** | **$$ P(W_1) $$** |
| :-----------: | :--------------: |
|      sol      |       0.5        |
|     lluvia      |       0.5        |

Notablemente, la probabilidad de que esté soleado ha disminuido del 80% en el tiempo $$t = 0$$ a solo el 50% en el tiempo $$t = 1$$. Este es un resultado directo de nuestro modelo de transición, que favorece la transición a días lluviosos sobre días soleados. Esto da lugar a una pregunta de seguimiento natural: ¿la probabilidad de estar en un estado en un paso de tiempo dado alguna vez converge? Abordaremos la respuesta a este problema en la siguiente sección.

# 8.1.2 Distribución estacionaria

Para resolver el problema planteado anteriormente, debemos calcular la **distribución estacionaria** del clima. Como sugiere el nombre, la distribución estacionaria es aquella que permanece igual después del paso del tiempo, es decir

$$P(W_{t+1}) = P(W_t)$$

Podemos calcular estas probabilidades convergentes de estar en un estado dado combinando la equivalencia anterior con la misma ecuación utilizada por el algoritmo mini-forward:

$$P(W_{t+1}) = P(W_t) = \sum_{w_t}P(W_{t+1} | w_t)P(w_t)$$

Para nuestro ejemplo meteorológico, esto nos da las siguientes dos ecuaciones:

<p></p>
$$P(W_t = sol) = P(W_{t+1} = sol | W_t = sol)P(W_t = sol) + P(W_{t+1} = sol | W_t = lluvia)P(W_t = lluvia)$$ $$= 0.6 \cdot P(W_t = sol) + 0.1 \cdot P(W_t = lluvia)$$

<p></p>
$$P(W_t = lluvia) = P(W_{t+1} = lluvia | W_t = sol)P(W_t = sol) + P(W_{t+1} = lluvia | W_t = lluvia)P(W_t = lluvia)$$ $$= 0.4 \cdot P(W_t = sol) + 0.9 \cdot P(W_t = lluvia)$$

Ahora tenemos dos ecuaciones con dos incógnitas. Para resolver, tenga en cuenta que la suma de estas probabilidades debe ser igual a uno, es decir

$$P(W_t = sol) + P(W_t = lluvia) = 1$$

Por lo tanto, si dejamos $$x = P(W_t = sol)$$ e $$y = P(W_t = lluvia)$$, podemos escribir el siguiente sistema de ecuaciones:

1. $$x + y = 1$$
2. $$0.6x + 0.1y = x$$
3. $$0.4x + 0.9y = y$$

Usando la primera ecuación, podemos sustituir $$y = 1 - x$$ en las otras dos, lo que nos da una sola ecuación en una incógnita:

$$0.6x + 0.1(1 - x) = x$$

Resolviendo esta ecuación se obtiene $$x = 1/5$$, y sustituyendo este valor en la primera ecuación se obtiene $$y = 4/5$$. Por lo tanto, nuestra distribución estacionaria es:

| **$$W$$** | **$$P(W)$$** |
| :-------: | :----------: |
|    sol    |     0.2      |
|   lluvia    |     0.8      |

A partir de este resultado, podemos concluir que a medida que avanzamos a través de nuestro algoritmo mini-forward y dejamos que el tiempo vaya al infinito, la probabilidad de que llueva converge al 80%. Este es otro resultado directo de nuestro modelo de transición, que favorece la transición a días lluviosos sobre días soleados.
