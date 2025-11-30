---
title: "6.5 D-Separación"
parent: 6. Redes Bayesianas
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.5 D-Separación

Una pregunta útil para hacer sobre un conjunto de variables aleatorias es si una variable es independiente de otra o no, o si una variable aleatoria es condicionalmente independiente de otra dada una tercera variable aleatoria. La representación de redes bayesianas de distribuciones de probabilidad conjunta nos da una forma de responder rápidamente a tales preguntas inspeccionando la estructura topológica del grafo.

Ya mencionamos que **un nodo es condicionalmente independiente de todos sus nodos ancestros en el grafo dados todos sus padres.**

Presentaremos los tres casos canónicos de redes bayesianas de tres nodos y dos bordes conectados, o triples, y las relaciones de independencia condicional que expresan.

## 6.5.1 Cadenas causales

<img src="{{ site.baseurl }}/assets/images/chain_free.PNG" alt="Cadena causal sin observaciones" />
*Figura 1: Cadena causal sin observaciones.*

<img src="{{ site.baseurl }}/assets/images/chain_observed.PNG" alt="Cadena causal con Y observada" />
*Figura 2: Cadena causal con Y observada.*

La Figura 1 es una configuración de tres nodos conocida como una **cadena causal**. Expresa la siguiente representación de la distribución conjunta sobre $$X$$, $$Y$$ y $$Z$$:

$$P(x, y, z) = P(z|y)P(y|x)P(x)$$

Es importante tener en cuenta que no se garantiza que $$X$$ y $$Z$$ sean independientes, como lo muestra el siguiente contraejemplo:

$$P(y|x) = 
\begin{cases} 
      1 & \text{si } x = y \\
      0 & \text{de lo contrario }
  \end{cases}$$

$$P(z|y) = 
\begin{cases} 
      1 & \text{si } z = y \\
      0 & \text{de lo contrario }
  \end{cases}$$

<p>
</p>
En este caso, $$P(z|x) = 1$$ si $$x = z$$ y $$0$$ de lo contrario, por lo que $$X$$ y $$Z$$ no son independientes.

<p>
</p>
Sin embargo, podemos afirmar que $$X \perp\!\!\!\perp Z | Y$$, como en la Figura 2. Recuerde que esta independencia condicional significa:
<p>
</p>

$$P(X | Z, Y) = P(X | Y)$$

Podemos probar esta afirmación de la siguiente manera:

$$P(X | Z, y) = \frac{P(X, Z, y)}{P(Z, y)}
= \frac{P(Z|y) P(y|X) P(X)}{\sum_{x} P(x, y, Z)}
= \frac{P(Z|y) P(y|X) P(X)}{P(Z|y) \sum_{x} P(y|x)P(x)}
= \frac{P(y|X) P(X)}{\sum_{x} P(y|x)P(x)}
= P(X|y)$$

<p>
</p>
Se puede utilizar una prueba análoga para mostrar lo mismo para el caso en el que $$X$$ tiene varios padres. Para resumir, en la configuración de cadena causal, $$X \perp\!\!\!\perp Z | Y$$.

## 6.5.2 Causa común

<img src="{{ site.baseurl }}/assets/images/cause_free.PNG" alt="Causa común sin observaciones" />

*Figura 3: Causa común sin observaciones.*

<img src="{{ site.baseurl }}/assets/images/cause_observed.PNG" alt="Causa común con Y observada" />

*Figura 4: Causa común con Y observada.*

Otra configuración posible para un triple es la **causa común**. Expresa la siguiente representación:

$$P(x, y, z) = P(x|y)P(z|y)P(y)$$

Al igual que con la cadena causal, podemos mostrar que no se garantiza que $$X$$ sea independiente de $$Z$$ con la siguiente distribución de contraejemplo:

$$P(x|y) = 
\begin{cases} 
      1 & \text{si } x = y \\
      0 & \text{de lo contrario }
  \end{cases}$$

$$P(z|y) = 
\begin{cases} 
      1 & \text{si } z = y \\
      0 & \text{de lo contrario }
  \end{cases}$$

<p>
</p>
Entonces $$P(x|z) = 1$$ si $$x = z$$ y $$0$$ de lo contrario, por lo que $$X$$ y $$Z$$ no son independientes.
<p>
</p>
Pero es cierto que $$X \perp\!\!\!\perp Z | Y$$. Es decir, $$X$$ y $$Z$$ son independientes si $$Y$$ se observa como en la Figura 4. Podemos mostrar esto de la siguiente manera:

$$P(X | Z, y) = \frac{P(X, Z, y)}{P(Z, y)} = \frac{P(X|y) P(Z|y) P(y)}{P(Z|y) P(y)} = P(X|y)$$

## 6.5.3 Efecto común

<img src="{{ site.baseurl }}/assets/images/effect_free.PNG" alt="Efecto común sin observaciones" />

*Figura 5: Efecto común sin observaciones.*

<img src="{{ site.baseurl }}/assets/images/effect_observed.PNG" alt="Efecto común con Y observada" />

*Figura 6: Efecto común con Y observada.*

Expresa la representación:

$$P(x, y, z) = P(y|x,z)P(x)P(z)$$

En la configuración que se muestra en la Figura 5, $$X$$ y $$Z$$ son independientes: $$X \perp\!\!\!\perp Z$$. Sin embargo, no son necesariamente independientes cuando se condicionan a $$Y$$ (Figura 6). Como ejemplo, supongamos que las tres son variables binarias. $$X$$ y $$Z$$ son verdaderos y falsos con igual probabilidad:

$$P(X=true) = P(X=false) = 0.5$$

$$P(Z=true) = P(Z=false) = 0.5$$

e $$Y$$ está determinado por si $$X$$ y $$Z$$ tienen el mismo valor:

$$P(Y | X, Z) = 
\begin{cases} 
  1 & \text{si } X = Z \text{ y } Y = true \\
  1 & \text{si } X \ne Z \text{ y } Y = false \\
  0 & \text{de lo contrario}
\end{cases}$$

Entonces $$X$$ y $$Z$$ son independientes si $$Y$$ no se observa. Pero si se observa $$Y$$, entonces conocer $$X$$ nos dirá el valor de $$Z$$, y viceversa. Entonces $$X$$ y $$Z$$ *no* son condicionalmente independientes dado $$Y$$.
<p>
</p>
El efecto común puede verse como "opuesto" a las cadenas causales y la causa común: se garantiza que $$X$$ y $$Z$$ son independientes si $$Y$$ no está condicionado. Pero cuando se condiciona a $$Y$$, $$X$$ y $$Z$$ pueden ser dependientes dependiendo de los valores de probabilidad específicos para $$P(Y | X, Z)$$.

Esta misma lógica se aplica al condicionar a los descendientes de $$Y$$ en el grafo. Si se observa uno de los nodos descendientes de $$Y$$, como en la Figura 7, no se garantiza que $$X$$ y $$Z$$ sean independientes.

<img src="{{ site.baseurl }}/assets/images/effect_children.PNG" alt="Efecto común con observaciones de hijos" />

*Figura 7: Efecto común con observaciones de hijos.*

## 6.5.4 Caso general y D-separación

Podemos usar los tres casos anteriores como bloques de construcción para ayudarnos a responder preguntas de independencia condicional en una red bayesiana arbitraria con más de tres nodos y dos bordes. Formulamos el problema de la siguiente manera:

<p>
</p>
**Dada una red bayesiana $$G$$, dos nodos $$X$$ e $$Y$$, y un conjunto (posiblemente vacío) de nodos $$\{Z_1, \dots Z_k\}$$ que representan variables observadas, ¿debe ser verdadera la siguiente afirmación: $$X \perp\!\!\!\perp Y | \{Z_1, \dots Z_k\}$$?**

<p>
</p>
La **D-separación** (separación dirigida) es una propiedad de la estructura del grafo de la red bayesiana que implica esta relación de independencia condicional y generaliza los casos que hemos visto anteriormente. Si un conjunto de variables $$Z_1, \cdots Z_k$$ d-separa $$X$$ e $$Y$$, entonces $$X \perp\!\!\!\perp Y | \{Z_1, \cdots Z_k\}$$ en todas las distribuciones posibles que pueden ser codificadas por la red bayesiana.

Comenzamos con un algoritmo que se basa en una noción de accesibilidad desde el nodo $$X$$ al nodo $$Y$$. (**Nota: ¡este algoritmo no es del todo correcto! Veremos cómo solucionarlo en un momento.**)

1. Sombree todos los nodos observados $$\{Z_1, \dots Z_k\}$$ en el grafo.
2. Si existe una ruta no dirigida desde $$X$$ e $$Y$$ que no está bloqueada por un nodo sombreado, $$X$$ e $$Y$$ están "conectados".
3. Si $$X$$ e $$Y$$ están conectados, no son condicionalmente independientes dados $$\{Z_1, \dots Z_k\}$$. De lo contrario, lo son.

Sin embargo, este algoritmo solo funciona si la red bayesiana no tiene una estructura de efecto común dentro del grafo, porque si existe, entonces dos nodos son "accesibles" cuando el nodo $$Y$$ en el efecto común está activado (observado). Para ajustar esto, llegamos al siguiente **algoritmo de d-separación**:

1. Sombree todos los nodos observados $$\{Z_1, \dots Z_k\}$$ en el grafo.
2. Enumere todas las rutas no dirigidas desde $$X$$ hasta $$Y$$.
3. Para cada ruta:
    1. Descomponga la ruta en triples (segmentos de 3 nodos).
    2. Si todos los triples están activos, esta ruta está activa y *d-conecta* $$X$$ con $$Y$$.
4. <p></p>Si ninguna ruta d-conecta $$X$$ e $$Y$$, entonces $$X \perp\!\!\!\perp Y | \{Z_1, \dots Z_k\}$$.

Cualquier ruta en un grafo de $$X$$ a $$Y$$ se puede descomponer en un conjunto de 3 nodos consecutivos y 2 bordes, cada uno de los cuales se llama triple. Un triple está activo o inactivo dependiendo de si el nodo central se observa o no. Si todos los triples en una ruta están activos, entonces la ruta está activa y *d-conecta* $$X$$ con $$Y$$, lo que significa que no se garantiza que $$X$$ sea condicionalmente independiente de $$Y$$ dados los nodos observados. Si todas las rutas de $$X$$ a $$Y$$ están inactivas, entonces $$X$$ e $$Y$$ son condicionalmente independientes dados los nodos observados.

---

Podemos enumerar todas las posibilidades de triples activos e inactivos utilizando los tres grafos canónicos que presentamos a continuación en las figuras.

**Triples activos**: 

<img src="{{ site.baseurl }}/assets/images/active.PNG" alt="Triples activos" />

**Triples inactivos**:

<img src="{{ site.baseurl }}/assets/images/inactive.PNG" alt="Triples inactivos" />

## 6.5.5 Ejemplos

Aquí hay algunos ejemplos de aplicación del algoritmo de $$d$$-separación:

<img src="{{ site.baseurl }}/assets/images/rbtt.png" alt="Ejemplo 1" />

Este grafo contiene los grafos canónicos de efecto común y cadena causal.

$$R \perp\!\!\!\perp B$$ -- Garantizado
<p>
</p>
$$R \perp\!\!\!\perp B | T$$ -- No garantizado
<p>
</p>
$$R \perp\!\!\!\perp B | T'$$ -- No garantizado
<p>
</p>
$$R \perp\!\!\!\perp T' | T$$ -- Garantizado

<img src="{{ site.baseurl }}/assets/images/lrbdtt.png" alt="Ejemplo 2" />

Este grafo contiene combinaciones de los tres grafos canónicos (¿puedes enumerarlos todos?).

$$L \perp\!\!\!\perp T' | T$$ -- Garantizado
<p>
</p>
$$L \perp\!\!\!\perp B$$ -- Garantizado
<p>
</p>
$$L \perp\!\!\!\perp B | T$$ -- No garantizado
<p>
</p>
$$L \perp\!\!\!\perp B | T'$$ -- No garantizado
<p>
</p>
$$L \perp\!\!\!\perp B | T, R$$ -- Garantizado

<img src="{{ site.baseurl }}/assets/images/rtds.png" alt="Ejemplo 3" />

Este grafo contiene combinaciones de los tres grafos canónicos.
<p>
</p>
$$T \perp\!\!\!\perp D$$ -- No garantizado
<p>
</p>
$$T \perp\!\!\!\perp D | R$$ -- Garantizado
<p>
</p>
$$T \perp\!\!\!\perp D | R, S$$ -- No garantizado
