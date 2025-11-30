---
title: "6.1 Resumen de probabilidad"
parent: 6. Redes Bayesianas
nav_order: 1
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.1 Resumen de probabilidad

Asumimos que ha aprendido los fundamentos de la probabilidad en CS70, por lo que estas notas asumirán una comprensión básica de los conceptos estándar en probabilidad como PDF, probabilidades condicionales, independencia e independencia condicional. Aquí proporcionamos un breve resumen de las reglas de probabilidad que utilizaremos.

Una **variable aleatoria** representa un evento cuyo resultado es desconocido. Una **distribución de probabilidad** es una asignación de pesos a los resultados. Las distribuciones de probabilidad deben cumplir las siguientes condiciones:

$$0 \leq P(\omega) \leq 1$$

$$\sum_{\omega}P(\omega) = 1$$

Por ejemplo, si $$A$$ es una variable binaria (solo puede tomar dos valores), entonces $$P(A = 0) = p$$ y $$P(A = 1) = 1 - p$$ para algún $$p \in [0,1]$$.

Usaremos la convención de que las letras mayúsculas se refieren a variables aleatorias y las letras minúsculas se refieren a algún resultado específico de esa variable aleatoria.

Usamos la notación $$P(A, B, C)$$ para denotar la **distribución conjunta** de las variables $$A, B, C$$. En las distribuciones conjuntas, el orden no importa, es decir, $$P(A, B, C) = P(C, B, A)$$.

Podemos expandir una distribución conjunta usando la **regla de la cadena**, a veces también conocida como regla del producto.

$$P(A, B) = P(A | B) P(B) = P(B | A) P(A)$$

$$P(A_1, A_2, \dots, A_k) = P(A_1) P(A_2 | A_1) \dots P(A_k | A_1, \dots, A_{k-1})$$

La **distribución marginal** de $$A, B$$ se puede obtener sumando todos los valores posibles que la variable $$C$$ puede tomar como $$P(A, B) = \sum_{c}P(A, B, C = c)$$. La distribución marginal de $$A$$ también se puede obtener como $$P(A) = \sum_{b} \sum_{c}P(A, B = b, C = c)$$. A veces también nos referiremos al proceso de marginación como "sumar".

Cuando hacemos operaciones en distribuciones de probabilidad, a veces obtenemos distribuciones que no necesariamente suman 1. Para solucionar esto, **normalizamos**: tomamos la suma de todas las entradas en la distribución y dividimos cada entrada por esa suma.

Las **probabilidades condicionales** asignan probabilidades a eventos condicionados a algunos hechos conocidos. Por ejemplo, $$P(A|B = b)$$ da la distribución de probabilidad de $$A$$ dado que sabemos que el valor de $$B$$ es igual a $$b$$. Las probabilidades condicionales se definen como:

$$P(A|B) = \frac{P(A, B)}{P(B)}.$$

Combinando la definición anterior de probabilidad condicional y la regla de la cadena, obtenemos la **Regla de Bayes**:

$$P(A | B) = \frac{P(B | A) P(A)}{P(B)}$$

Para escribir que las variables aleatorias $$A$$ y $$B$$ son **mutuamente independientes**, escribimos $$A \perp\!\!\!\perp B$$. Esto es equivalente a $$B \perp\!\!\!\perp A$$.

Cuando $$A$$ y $$B$$ son mutuamente independientes, $$P(A, B) = P(A) P(B)$$. Un ejemplo en el que puede pensar son dos lanzamientos de monedas independientes. Es posible que esté familiarizado con la independencia mutua simplemente como "independencia" en otros cursos. Podemos derivar de la ecuación anterior y la regla de la cadena que $$P(A | B) = P(A)$$ y $$P(B | A) = P(B)$$.

Para escribir que las variables aleatorias $$A$$ y $$B$$ son **condicionalmente independientes** dada otra variable aleatoria $$C$$, escribimos $$A \perp\!\!\!\perp B | C$$. Esto también es equivalente a $$B \perp\!\!\!\perp A | C$$.

Si $$A$$ y $$B$$ son condicionalmente independientes dado $$C$$, entonces $$P(A, B | C) = P(A | C) P(B | C)$$. Esto significa que si tenemos conocimiento sobre el valor de $$C$$, entonces $$B$$ y $$A$$ no se afectan entre sí. Equivalente a la definición anterior de independencia condicional son las relaciones $$P(A | B, C) = P(A | C)$$ y $$P(B | A, C) = P(B | C)$$. ¡Observe cómo estas tres ecuaciones son equivalentes a las tres ecuaciones para la independencia mutua, solo con un condicional agregado en $$C$$!
