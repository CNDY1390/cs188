---
title: 7.1 Utilidades
parent: 7. Redes de decisión y VPIs
nav_order: 1
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 7.1 Utilidades

A lo largo de nuestra discusión sobre los agentes racionales, el concepto de utilidad surgió repetidamente. En los juegos, por ejemplo, los valores de utilidad generalmente están integrados en el juego, y los agentes usan estos valores de utilidad para seleccionar una acción. Ahora discutiremos qué es necesario para generar una función de utilidad viable.

Los agentes racionales deben seguir el **principio de máxima utilidad**: siempre deben seleccionar la acción que maximice su utilidad esperada. Sin embargo, obedecer este principio solo beneficia a los agentes que tienen **preferencias racionales**. Para construir un ejemplo de preferencias irracionales, digamos que existen 3 objetos, $$A$$, $$B$$ y $$C$$, y nuestro agente está actualmente en posesión de $$A$$. Digamos que nuestro agente tiene el siguiente conjunto de preferencias irracionales:

- Nuestro agente prefiere $$B$$ a $$A$$ más \$1
- Nuestro agente prefiere $$C$$ a $$B$$ más \$1
- Nuestro agente prefiere $$A$$ a $$C$$ más \$1

Un agente malicioso en posesión de $$B$$ y $$C$$ puede intercambiar a nuestro agente $$B$$ por $$A$$ más un dólar, luego $$C$$ por $$B$$ más un dólar, luego $$A$$ nuevamente por $$C$$ más un dólar. ¡Nuestro agente acaba de perder \$3 por nada! De esta manera, nuestro agente puede verse obligado a renunciar a todo su dinero en un ciclo interminable y de pesadilla.

Ahora definamos correctamente el lenguaje matemático de las preferencias:

- Si un agente prefiere recibir un premio $$A$$ a recibir un premio $$B$$, esto se escribe $$A \succ B$$
- Si un agente es indiferente entre recibir $$A$$ o $$B$$, esto se escribe como $$A \sim B$$
- Una **lotería** es una situación con diferentes premios que resultan con diferentes probabilidades. Para denotar una lotería donde se recibe $$A$$ con probabilidad $$p$$ y se recibe $$B$$ con probabilidad $$(1-p)$$, escribimos:

  $$L = [p, A; (1-p), B]$$

Para que un conjunto de preferencias sea racional, deben seguir los cinco **Axiomas de Racionalidad**:

- **Ordenabilidad**:  
  $$(A \succ B) \vee (B \succ A) \vee (A \sim B)$$  
  Un agente racional debe preferir uno de $$A$$ o $$B$$, o ser indiferente entre los dos.
  
- **Transitividad**:  
  $$(A \succ B) \wedge (B \succ C) \Rightarrow (A \succ C)$$  
  Si un agente racional prefiere $$A$$ a $$B$$ y $$B$$ a $$C$$, entonces prefiere $$A$$ a $$C$$.

- **Continuidad**:  
  $$A \succ B \succ C \Rightarrow \exists p \: [p, A; (1-p), C] \sim B$$  
  Si un agente racional prefiere $$A$$ a $$B$$ pero $$B$$ a $$C$$, entonces es posible construir una lotería $$L$$ entre $$A$$ y $$C$$ tal que el agente sea indiferente entre $$L$$ y $$B$$ con la selección apropiada de $$p$$.

- **Sustituibilidad**:  
  $$A \sim B \Rightarrow [p, A; (1-p), C] \sim [p, B; (1-p), C]$$  
  Un agente racional indiferente entre dos premios $$A$$ y $$B$$ también es indiferente entre dos loterías cualesquiera que solo difieran en sustituciones de $$A$$ por $$B$$ o $$B$$ por $$A$$.

- **Monotonicidad**:  
  $$A \succ B \Rightarrow (p \geq q) \Leftrightarrow [p, A; (1-p), B] \succeq [q, A; (1-q), B]$$  
  Si un agente racional prefiere $$A$$ sobre $$B$$, entonces, dada una elección entre loterías que involucran solo $$A$$ y $$B$$, el agente prefiere la lotería que asigna la mayor probabilidad a $$A$$.

Si un agente satisface los cinco axiomas, entonces se garantiza que el comportamiento del agente se puede describir como una maximización de la utilidad esperada. Más específicamente, esto implica que existe una **función de utilidad** de valor real $$U$$ que, cuando se implemente, asignará mayores utilidades a los premios preferidos, y también que la utilidad de una lotería es el valor esperado de la utilidad del premio resultante de la lotería. Estas dos declaraciones se pueden resumir en dos equivalencias matemáticas concisas:

$$U(A) \geq U(B) \Leftrightarrow A \succeq B$$

$$U([p_1, S_1; ... ;p_n, S_n]) = \sum_i p_i U(S_i)$$

Si se cumplen estas restricciones y se realiza una elección adecuada del algoritmo, se garantiza que el agente que implementa dicha función de utilidad se comportará de manera óptima. Discutamos las funciones de utilidad con mayor detalle con un ejemplo concreto. Considere la siguiente lotería:

$$L = [0.5, \$0; 0.5, \$1000]$$

Esto representa una lotería en la que recibe \$1000 con una probabilidad de 0.5 y \$0 con una probabilidad de 0.5. Ahora considere tres agentes $$A_1$$, $$A_2$$ y $$A_3$$ que tienen funciones de utilidad $$U_1(\$x) = x$$, $$U_2(\$x) = \sqrt{x}$$ y $$U_3(\$x) = x^2$$ respectivamente. Si cada uno de los tres agentes se enfrentara a una elección entre participar en la lotería y recibir un pago fijo de \$500, ¿cuál elegirían? Las utilidades respectivas para cada agente de participar en la lotería y aceptar el pago fijo se enumeran en la siguiente tabla:

| **Agente** | **Lotería** | **Pago Fijo** |
|-----------|-------------|------------------|
| 1         | 500         | 500              |
| 2         | 15.81       | 22.36            |
| 3         | 500000      | 250000           |

Estos valores de utilidad para las loterías se calcularon de la siguiente manera, haciendo uso de la ecuación (2) anterior:

$$U_1(L) = U_1([0.5, \$0; 0.5, \$1000]) = 0.5 \cdot U_1(\$1000) + 0.5 \cdot U_1(\$0) = 0.5 \cdot 1000 + 0.5 \cdot 0 = \boxed{500}$$

$$U_2(L) = U_2([0.5, \$0; 0.5, \$1000]) = 0.5 \cdot U_2(\$1000) + 0.5 \cdot U_2(\$0) = 0.5 \cdot \sqrt{1000} + 0.5 \cdot \sqrt{0} = \boxed{15.81}$$

$$U_3(L) = U_1([0.5, \$0; 0.5, \$1000]) = 0.5 \cdot U_3(\$1000) + 0.5 \cdot U_3(\$0) = 0.5 \cdot 1000^2 + 0.5 \cdot 0^2 = \boxed{500000}$$

Con estos resultados, podemos ver que el agente $$A_1$$ es indiferente entre participar en la lotería y recibir el pago fijo (las utilidades para ambos casos son idénticas). Tal agente se conoce como **neutral al riesgo**. De manera similar, el agente $$A_2$$ prefiere el pago fijo a la lotería y se conoce como **adverso al riesgo** y el agente $$A_3$$ prefiere la lotería al pago fijo y se conoce como **buscador de riesgo**.
