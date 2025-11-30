---
title: 8.4 Filtrado de partículas
parent: 8. HMMs
nav_order: 4
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 8.4 Filtrado de partículas

Recuerde que con las redes bayesianas, cuando ejecutar una inferencia exacta era demasiado costoso computacionalmente, usar una de las técnicas de muestreo que discutimos era una alternativa viable para aproximar eficientemente la(s) distribución(es) de probabilidad deseada(s) que queríamos. Los Modelos Ocultos de Markov tienen el mismo inconveniente: el tiempo que lleva ejecutar una inferencia exacta con el algoritmo forward escala con el número de valores en los dominios de las variables aleatorias. Esto era aceptable en nuestra formulación actual del problema meteorológico donde el clima solo puede tomar 2 valores, $$W_i \in \{sol, lluvia\}$$, pero digamos que en cambio quisiéramos ejecutar una inferencia para calcular la distribución de la temperatura real en un día determinado al décimo de grado más cercano.

El análogo del Modelo Oculto de Markov al muestreo de redes bayesianas se llama **filtrado de partículas**, e implica simular el movimiento de un conjunto de partículas a través de un grafo de estado para aproximar la distribución de probabilidad (creencia) de la variable aleatoria en cuestión. Esto resuelve la misma pregunta que el Algoritmo Forward: nos da una aproximación de $$P(X_N \mid e_{1:N})$$.

En lugar de almacenar una tabla de probabilidad completa que asigne cada estado a su probabilidad de creencia, almacenaremos una lista de $$n$$ **partículas**, donde cada partícula se encuentra en uno de los $$d$$ estados posibles en el dominio de nuestra variable aleatoria dependiente del tiempo. Típicamente, $$n$$ es significativamente menor que $$d$$ (denotado simbólicamente como $$n << d$$) pero aún lo suficientemente grande como para producir aproximaciones significativas; de lo contrario, la ventaja de rendimiento del filtrado de partículas se vuelve insignificante. Las partículas son solo el nombre de las muestras en este algoritmo.

Nuestra creencia de que una partícula está en cualquier estado dado en cualquier paso de tiempo dado depende completamente del número de partículas en ese estado en ese paso de tiempo en nuestra simulación. Por ejemplo, digamos que de hecho queríamos simular la distribución de creencias de la temperatura $$T$$ en algún día $$i$$ y asumir por simplicidad que esta temperatura solo puede tomar valores enteros en el rango $$[10, 20]$$ ($$d = 11$$ estados posibles). Supongamos además que tenemos $$n = 10$$ partículas, que toman los siguientes valores en el paso de tiempo $$i$$ de nuestra simulación:

$$[15, 12, 12, 10, 18, 14, 12, 11, 11, 10]$$

Tomando recuentos de cada temperatura que aparece en nuestra lista de partículas y dividiendo por el número total de partículas, podemos generar nuestra distribución empírica deseada para la temperatura en el tiempo $$i$$:

| **$$T_i $$**    | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  | 18  | 19  | 20  |
|------------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **$$B(T_i) $$** | 0.2 | 0.2 | 0.3 | 0   | 0.1 | 0.1 | 0   | 0   | 0.1 | 0   | 0   |

Ahora que hemos visto cómo recuperar una distribución de creencias de una lista de partículas, todo lo que queda por discutir es cómo generar dicha lista para un paso de tiempo de nuestra elección.

## 8.4.1 Simulación de filtrado de partículas

La simulación de filtrado de partículas comienza con la inicialización de partículas, que se puede hacer de manera bastante flexible: podemos muestrear partículas aleatoriamente, uniformemente o de alguna distribución inicial. Una vez que hemos muestreado una lista inicial de partículas, la simulación toma una forma similar al algoritmo forward, con una actualización de lapso de tiempo seguida de una actualización de observación en cada paso de tiempo:

- **Actualización de lapso de tiempo**: actualice el valor de cada partícula de acuerdo con el modelo de transición. Para una partícula en el estado $$t_i$$, muestree el valor actualizado de la distribución de probabilidad dada por $$P(T_{i+1} \mid t_i)$$. Tenga en cuenta la similitud de la actualización de lapso de tiempo con el muestreo previo con redes bayesianas, ya que la frecuencia de partículas en cualquier estado dado refleja las probabilidades de transición.
- **Actualización de observación**: durante la actualización de observación para el filtrado de partículas, usamos el modelo de sensor $$P(F_i \mid T_i)$$ para ponderar cada partícula de acuerdo con la probabilidad dictada por la evidencia observada y el estado de la partícula. Específicamente, para una partícula en el estado $$t_i$$ con lectura del sensor $$f_i$$, asigne un peso de $$P(f_i \mid t_i)$$. El algoritmo para la actualización de observación es el siguiente:
  1. Calcule los pesos de todas las partículas como se describe anteriormente.
  2. Calcule el peso total para cada estado.
  3. Si la suma de todos los pesos en todos los estados es 0, reinicialice todas las partículas.
  4. De lo contrario, normalice la distribución de pesos totales sobre los estados y vuelva a muestrear su lista de partículas de esta distribución.

Tenga en cuenta la similitud de la actualización de observación con la ponderación de verosimilitud, donde nuevamente bajamos la ponderación de las muestras en función de nuestra evidencia.

Veamos si podemos entender este proceso un poco mejor con un ejemplo. Defina un modelo de transición para nuestro escenario meteorológico utilizando la temperatura como la variable aleatoria dependiente del tiempo de la siguiente manera: para un estado de temperatura particular, puede permanecer en el mismo estado o pasar a un estado a un grado de distancia, dentro del rango $$[10, 20]$$. De los posibles estados resultantes, la probabilidad de pasar al más cercano a 15 es del 80% y los estados resultantes restantes dividen uniformemente el 20% de probabilidad restante entre ellos.

Nuestra lista de partículas de temperatura fue la siguiente:

$$[15, 12, 12, 10, 18, 14, 12, 11, 11, 10]$$

Para realizar una actualización de lapso de tiempo para la primera partícula en esta lista de partículas, que está en el estado $$T_i = 15$$, necesitamos el modelo de transición correspondiente:

| **$$T_{i+1}$$**    | 14  | 15  | 16  |
|----------------------|-----|-----|-----|
| **$$P(T_{i+1} \mid T_i = 15)$$** | 0.1 | 0.8 | 0.1 |

En la práctica, asignamos un rango diferente de valores para cada valor en el dominio de $$T_{i+1}$$ de tal manera que juntos los rangos abarcan completamente el intervalo $$[0, 1)$$ sin superposición. Para el modelo de transición anterior, los rangos son los siguientes:
1. El rango para $$T_{i+1} = 14$$ es $$0 \leq r < 0.1$$.
2. El rango para $$T_{i+1} = 15$$ es $$0.1 \leq r < 0.9$$.
3. El rango para $$T_{i+1} = 16$$ es $$0.9 \leq r < 1$$.

Para volver a muestrear nuestra partícula en el estado $$T_i = 15$$, simplemente generamos un número aleatorio en el rango $$[0, 1)$$ y vemos en qué rango cae. Por lo tanto, si nuestro número aleatorio es $$r = 0.467$$, entonces la partícula en $$T_i = 15$$ permanece en $$T_{i+1} = 15$$ ya que $$0.1 \leq r < 0.9$$. Ahora considere la siguiente lista de 10 números aleatorios en el intervalo $$[0, 1)$$:

$$[0.467, 0.452, 0.583, 0.604, 0.748, 0.932, 0.609, 0.372, 0.402, 0.026]$$

Si usamos estos 10 valores como el valor aleatorio para volver a muestrear nuestras 10 partículas, nuestra nueva lista de partículas después de la actualización completa del lapso de tiempo debería verse así:

$$[15, 13, 13, 11, 17, 15, 13, 12, 12, 10]$$

¡Verifique esto usted mismo! La lista de partículas actualizada da lugar a la distribución de creencias actualizada correspondiente $$B(T_{i+1})$$:

| $$T_i$$    | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  | 18  | 19  | 20  |
|------------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| $$B(T_{i+1})$$ | 0.1 | 0.1 | 0.2 | 0.3 | 0   | 0.2 | 0   | 0.1 | 0   | 0   | 0   |

Comparando nuestra distribución de creencias actualizada $$B(T_{i+1})$$ con nuestra distribución de creencias inicial $$B(T_i)$$, podemos ver que, como tendencia general, las partículas tienden a converger hacia una temperatura de $$T = 15$$.

<p>
</p>

A continuación, realicemos la actualización de observación, asumiendo que nuestro modelo de sensor $$P(F_i | T_i)$$ establece que la probabilidad de un pronóstico correcto $$f_i = t_i$$ es del 80%, con una probabilidad uniforme del 2% de que el pronóstico prediga cualquiera de los otros 10 estados. Suponiendo un pronóstico de $$F_{i+1} = 13$$, los pesos de nuestras 10 partículas son los siguientes:
<p>
</p>

| **Partícula** | $$p_1$$ | $$p_2$$ | $$p_3$$ | $$p_4$$ | $$p_5$$ | $$p_6$$ | $$p_7$$ | $$p_8$$ | $$p_9$$ | $$p_{10}$$ |
|--------------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|--------------|
| **Estado**    | 15        | 13        | 13        | 11        | 17        | 15        | 13        | 12        | 12        | 10           |
| **Peso**   | 0.02      | 0.8       | 0.8       | 0.02      | 0.02      | 0.02      | 0.8       | 0.02      | 0.02      | 0.02          |

Luego agregamos pesos por estado:

| **Estado** | 10   | 11   | 12   | 13  | 15   | 17  |
|-----------|------|------|------|-----|------|-----|
| **Peso**| 0.02 | 0.02 | 0.04 | 2.4 | 0.04 | 0.02 |

Sumando los valores de todos los pesos se obtiene una suma de 2.54, y podemos normalizar nuestra tabla de pesos para generar una distribución de probabilidad dividiendo cada entrada por esta suma:

| **Estado**          | 10     | 11     | 12     | 13    | 15     | 17    |
|--------------------|--------|--------|--------|-------|--------|-------|
| **Peso**         | 0.02   | 0.02   | 0.04   | 2.4   | 0.04   | 0.02  |
| **Peso Normalizado** | 0.0079 | 0.0079 | 0.0157 | 0.9449 | 0.0157 | 0.0079 |

El paso final es volver a muestrear de esta distribución de probabilidad, utilizando la misma técnica que usamos para volver a muestrear durante la actualización de lapso de tiempo. Digamos que generamos 10 números aleatorios en el rango $$[0, 1)$$ con los siguientes valores:

$$[0.315, 0.829, 0.304, 0.368, 0.459, 0.891, 0.282, 0.980, 0.898, 0.341]$$

Esto produce una lista de partículas remuestreada de la siguiente manera:

$$[13, 13, 13, 13, 13, 13, 13, 15, 13, 13]$$

Con la nueva distribución de creencias final correspondiente:

| $$T_i$$    | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  | 18  | 19  | 20  |
|------------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| $$B(T_{i+1})$$ | 0   | 0   | 0   | 0.9 | 0   | 0.1 | 0   | 0   | 0   | 0   | 0   |

Observe que nuestro modelo de sensor codifica que nuestra predicción meteorológica es muy precisa con una probabilidad del 80%, y que nuestra nueva lista de partículas es consistente con esto ya que la mayoría de las partículas se vuelven a muestrear para ser $$T_{i+1} = 13$$.
