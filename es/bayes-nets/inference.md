---
title: '6.2 Inferencia probabilística'
parent: 6. Redes Bayesianas
nav_order: 2
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.2 Inferencia probabilística

En inteligencia artificial, a menudo queremos modelar las relaciones entre varios eventos no deterministas. Si el clima predice un 40% de probabilidad de lluvia, ¿debo llevar mi paraguas? ¿Cuántas bolas de helado debo pedir si cuantas más pido, más probabilidades tengo de que se me caiga todo? Si hubo un accidente hace 15 minutos en la autopista en mi ruta al Oracle Arena para ver el partido de los Warriors, ¿debo salir ahora o en 30 minutos? Todas estas preguntas (y muchas más) se pueden responder con **inferencia probabilística**.

En secciones anteriores de esta clase, modelamos el mundo como existente en un estado específico que siempre se conoce. Durante las próximas semanas, utilizaremos un nuevo modelo donde cada estado posible para el mundo tiene su propia probabilidad. Por ejemplo, podríamos construir un modelo meteorológico, donde el estado consiste en la estación, la temperatura y el clima. Nuestro modelo podría decir que $$P(invierno, 35°, nublado) = 0.023$$. Este número representa la probabilidad del resultado específico de que sea invierno, 35° y nublado.

Más precisamente, nuestro modelo es una **distribución conjunta**, es decir, una tabla de probabilidades que captura la probabilidad de cada **resultado** posible, también conocido como **asignación** de variables. Como ejemplo, considere la siguiente tabla:

| **Estación** | **Temperatura** | **Clima** | **Probabilidad** |
| ---------- | --------------- | ----------- | --------------- |
| verano     | caliente        | sol         | 0.30            |
| verano     | caliente        | lluvia      | 0.05            |
| verano     | frío            | sol         | 0.10            |
| verano     | frío            | lluvia      | 0.05            |
| invierno   | caliente        | sol         | 0.10            |
| invierno   | caliente        | lluvia      | 0.05            |
| invierno   | frío            | sol         | 0.15            |
| invierno   | frío            | lluvia      | 0.20            |

Este modelo nos permite responder preguntas que podrían ser de nuestro interés, por ejemplo:

<p></p>
1.¿Cuál es la probabilidad de que esté soleado? $$P(W = sol)$$
<p></p>
2.¿Cuál es la distribución de probabilidad para el clima, dado que sabemos que es invierno? $$P(W | S = invierno)$$
<p></p>
3.¿Cuál es la probabilidad de que sea invierno, dado que sabemos que está lloviendo y hace frío? $$P(S = invierno | T = frío, W = lluvia)$$
<p></p>
4.¿Cuál es la distribución de probabilidad para el clima y la estación dado que sabemos que hace frío? $$P(S, W | T = frío)$$

## Inferencia por enumeración

<p></p>
Dada una PDF conjunta, podemos calcular trivialmente cualquier distribución de probabilidad deseada $$P(Q_1...Q_m| e_1...e_n)$$ utilizando un procedimiento simple e intuitivo conocido como **inferencia por enumeración**, para el cual definimos tres tipos de variables con las que trataremos:
<p></p>
1.**Variables de consulta** $$Q_i$$, que son desconocidas y aparecen en el lado izquierdo del condicional ($$|$$) en la distribución de probabilidad deseada.
<p></p>
2.**Variables de evidencia** $$e_i$$, que son variables observadas cuyos valores se conocen y aparecen en el lado derecho del condicional ($$|$$) en la distribución de probabilidad deseada.
<p></p>
3.**Variables ocultas**, que son valores presentes en la distribución conjunta general pero no en la distribución deseada.

En Inferencia por Enumeración, seguimos el siguiente algoritmo:

1. Recopile todas las filas consistentes con las variables de evidencia observadas.
2. Sume (margine) todas las variables ocultas.
3. Normalice la tabla para que sea una distribución de probabilidad (es decir, los valores suman 1).
<p></p>
Por ejemplo, si quisiéramos calcular $$P(W | S = invierno)$$ usando la distribución conjunta anterior, seleccionaríamos las cuatro filas donde $$S$$ es invierno, luego sumaríamos sobre $$T$$ y normalizaríamos. Esto produce la siguiente tabla de probabilidad:

| **W** | **S**  | **Suma no normalizada**   | **Probabilidad**                |
| ----- | ------ | ---------------------- | ------------------------------ |
| sol   | invierno | $$0.10 + 0.15 = 0.25$$ | $$0.25 / (0.25 + 0.25) = 0.5$$ |
| lluvia  | invierno | $$0.05 + 0.20 = 0.25$$ | $$0.25 / (0.25 + 0.25) = 0.5$$ |

<p></p>
Por lo tanto, $$P(W = sol | S = invierno) = 0.5$$ y $$P(W = lluvia | S = invierno) = 0.5$$, y aprendemos que en invierno hay un 50% de probabilidad de sol y un 50% de probabilidad de lluvia.

Siempre que tengamos la tabla PDF conjunta, la inferencia por enumeración (IBE) se puede utilizar para calcular cualquier distribución de probabilidad deseada, incluso para múltiples variables de consulta $$Q_1...Q_m$$.
