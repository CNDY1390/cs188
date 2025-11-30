---
title: "6.7 Inferencia aproximada en redes bayesianas: Muestreo"
parent: 6. Redes Bayesianas
nav_order: 7
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.7 Inferencia aproximada en redes bayesianas: Muestreo

Un enfoque alternativo para el razonamiento probabilístico es calcular implícitamente las probabilidades de nuestra consulta simplemente contando muestras. Esto no arrojará la solución exacta, como en IBE o Eliminación de variables, pero esta inferencia aproximada a menudo es lo suficientemente buena, especialmente cuando se tienen en cuenta los ahorros masivos en el cálculo.

Por ejemplo, supongamos que quisiéramos calcular $$P(+t \mid +e)$$. Si tuviéramos una máquina mágica que pudiera generar muestras de nuestra distribución, podríamos recopilar todas las muestras para las cuales $$E = +e$$, y luego calcular la fracción de esas muestras para las cuales $$T = +t$$. Podríamos calcular fácilmente cualquier inferencia que quisiéramos simplemente mirando las muestras. Veamos algunos métodos diferentes para generar muestras.

## 6.7.1 Muestreo previo

Dado un modelo de red bayesiana, podemos escribir fácilmente un simulador. Por ejemplo, considere las CPT que se dan a continuación para el modelo simplificado con solo dos variables T y C.

<img src="{{ site.baseurl }}/assets/images/TC_model.PNG" alt="Modelo TC" />

Un simulador simple en Python se escribiría de la siguiente manera:

```python
import random

def get_t():
    if random.random() < 0.99:
        return True
    return False

def get_c(t):
    if t and random.random() < 0.95:
        return True
    return False

def get_sample():
    t = get_t()
    c = get_c(t)
    return [t, c]
```

Llamamos a este enfoque simple **muestreo previo**. La desventaja de este enfoque es que puede requerir la generación de una gran cantidad de muestras para realizar análisis de escenarios poco probables. Si quisiéramos calcular $$P(C \mid -t)$$, tendríamos que tirar el 99% de nuestras muestras.

## 6.7.2 Muestreo de rechazo

Una forma de mitigar el problema mencionado anteriormente es modificar nuestro procedimiento para rechazar temprano cualquier muestra inconsistente con nuestra evidencia. Por ejemplo, para la consulta $$P(C \mid -t)$$, evitaríamos generar un valor para C a menos que t sea falso. Esto todavía significa que tenemos que tirar la mayoría de nuestras muestras, pero al menos las muestras malas que generamos toman menos tiempo en crearse. Llamamos a este enfoque **muestreo de rechazo**.

Estos dos enfoques funcionan por la misma razón: cualquier muestra válida ocurre con la misma probabilidad especificada en la PDF conjunta.

## 6.7.3 Ponderación de verosimilitud

Un enfoque más exótico es la **ponderación de verosimilitud**, que garantiza que nunca generemos una muestra mala. En este enfoque, establecemos manualmente todas las variables iguales a la evidencia en nuestra consulta. Por ejemplo, si quisiéramos calcular $$P(C \mid -t)$$, simplemente declararíamos que $$t$$ es falso. El problema aquí es que esto puede producir muestras que son inconsistentes con la distribución correcta.

Si simplemente forzamos que algunas variables sean iguales a la evidencia, entonces nuestras muestras ocurren con probabilidad solo igual a los productos de las CPT de las variables sin evidencia. Esto significa que la PDF conjunta no tiene garantía de ser correcta (aunque puede serlo para algunos casos como nuestra red bayesiana de dos variables). En cambio, si tenemos variables muestreadas $$Z_1$$ a $$Z_p$$ y variables de evidencia fijas $$E_1$$ a $$E_m $$, una muestra viene dada por la probabilidad $$P(Z_1 \ldots Z_p, E_1 \ldots E_m) = \prod_{i=1}^p P(Z_i \mid \text{Padres}(Z_i))$$. Lo que falta es que la probabilidad de una muestra no incluye todas las probabilidades de $$P(E_i \mid \text{Padres}(E_i))$$, es decir, no todas las CPT participan.

La ponderación de verosimilitud resuelve este problema utilizando un peso para cada muestra, que es la probabilidad de las variables de evidencia dadas las variables muestreadas. Es decir, en lugar de contar todas las muestras por igual, podemos definir un peso $$w_j$$ para la muestra $$j$$ que refleje qué tan probables son los valores observados para las variables de evidencia, dados los valores muestreados. De esta manera, nos aseguramos de que cada CPT participe. Para hacer esto, iteramos a través de cada variable en la red bayesiana, como lo hacemos para el muestreo normal, muestreando un valor si la variable no es una variable de evidencia, o cambiando el peso de la muestra si la variable es evidencia.

Por ejemplo, supongamos que queremos calcular $$P(T \mid +c, +e)$$. Para la $$j$$-ésima muestra, realizaríamos el siguiente algoritmo:

- Establecer $$w_j$$ en 1.0, y $$c = \text{true}$$ y $$e = \text{true}$$.
- Para $$T$$: Esta no es una variable de evidencia, por lo que muestreamos $$t_j$$ de $$P(T)$$.
- Para $$C$$: Esta es una variable de evidencia, por lo que multiplicamos el peso de la muestra por $$P(+c \mid t_j)$$, es decir, $$w_j = w_j \cdot P(+c \mid t_j)$$.
- Para $$S$$: muestrear $$s_j$$ de $$P(S \mid t_j)$$.
- Para $$E$$: multiplicar el peso de la muestra por $$P(+e \mid +c, s_j)$$, es decir, $$w_j = w_j \cdot P(+e \mid +c, s_j)$$.

Luego, cuando realizamos el proceso de conteo habitual, ponderamos la muestra $$j$$ por $$w_j$$ en lugar de 1, donde $$0 \leq w_j \leq 1$$. Este enfoque funciona porque en los cálculos finales de las probabilidades, los pesos sirven efectivamente para reemplazar las CPT faltantes. En efecto, nos aseguramos de que la probabilidad ponderada de cada muestra esté dada por $$P(z_1 \ldots z_p, e_1 \ldots e_m) = \left[\prod_{i=1}^p P(z_i \mid \text{Padres}(z_i))\right] \cdot \left[\prod_{i=1}^m P(e_i \mid \text{Padres}(e_i))\right]$$. El pseudocódigo para la ponderación de verosimilitud se proporciona a continuación.

<img src="{{ site.baseurl }}/assets/images/LikelihoodWeighting.png" alt="Ponderación de verosimilitud" />

Para nuestros tres métodos de muestreo (muestreo previo, muestreo de rechazo y ponderación de verosimilitud), podemos obtener cantidades crecientes de precisión generando muestras adicionales. Sin embargo, de los tres, la ponderación de verosimilitud es la más eficiente computacionalmente, por razones que escapan al alcance de este curso.

## 6.7.4 Muestreo de Gibbs

El **Muestreo de Gibbs** es un cuarto enfoque para el muestreo. En este enfoque, primero establecemos todas las variables en algún valor totalmente aleatorio (sin tener en cuenta ninguna CPT). Luego elegimos repetidamente una variable a la vez, borramos su valor y la volvemos a muestrear dados los valores asignados actualmente a todas las demás variables.

Para el ejemplo $$T, C, S, E$$ anterior, podríamos asignar $$t = \text{true}$$, $$c = \text{true}$$, $$s = \text{false}$$ y $$e = \text{true}$$. Luego elegimos una de nuestras cuatro variables para volver a muestrear, digamos $$S$$, y la borramos. Luego elegimos una nueva variable de la distribución $$P(S \mid +t, +c, +e)$$. Esto requiere que conozcamos esta distribución condicional. Resulta que podemos calcular fácilmente la distribución de cualquier variable individual dadas todas las demás variables. Más específicamente, $$P(S \mid T, C, E)$$ se puede calcular solo usando las CPT que conectan S con sus vecinos. Por lo tanto, en una red bayesiana típica, donde la mayoría de las variables tienen solo un pequeño número de vecinos, podemos precalcular las distribuciones condicionales para cada variable dados todos sus vecinos en tiempo lineal.

No probaremos esto, pero si repetimos este proceso suficientes veces, nuestras muestras posteriores eventualmente convergerán a la distribución correcta a pesar de que podamos comenzar desde una asignación de valores de baja probabilidad. Si tiene curiosidad, hay algunas advertencias más allá del alcance del curso que puede leer en la sección Modos de falla del artículo de Wikipedia para el Muestreo de Gibbs.

El pseudocódigo para el Muestreo de Gibbs se proporciona a continuación.

<img src="{{ site.baseurl }}/assets/images/Gibbs.png" alt="Muestreo de Gibbs" />
