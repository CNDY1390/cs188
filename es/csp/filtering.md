---
title: 2.3 Filtrado
parent: 2. CSPs
nav_order: 3
layout: page
lang: es
header-includes:
    \pagenumbering{gobble}
---

# 2.3 Filtrado

La primera mejora en el rendimiento de CSP que consideraremos es el **filtrado**, que verifica si podemos podar los dominios de variables no asignadas con anticipación eliminando valores que sabemos que resultarán en retrocesos. Un método ingenuo para el filtrado es la **verificación hacia adelante (forward checking)**, que cada vez que se asigna un valor a una variable $$X_i$$, poda los dominios de las variables no asignadas que comparten una restricción con $$X_i$$ que violaría la restricción si se asignara. Cada vez que se asigna una nueva variable, podemos ejecutar la verificación hacia adelante y podar los dominios de las variables no asignadas adyacentes a la variable recién asignada en el grafo de restricciones. Considere nuestro ejemplo de coloreado de mapas, con variables no asignadas y sus valores potenciales:

<img src="{{ site.baseurl }}/assets/images/forward-checking.png" alt="Ejemplo de verificación hacia adelante" />

Observe cómo a medida que asignamos $$WA = \text{rojo}$$ y luego $$Q = \text{verde}$$, el tamaño de los dominios para $$NT$$, $$NSW$$ y $$SA$$ (estados adyacentes a $$WA$$, $$Q$$ o ambos) disminuye en tamaño a medida que se eliminan los valores. La idea de la verificación hacia adelante se puede generalizar en el principio de **consistencia de arco**. Para la consistencia de arco, interpretamos cada borde no dirigido del grafo de restricciones para un CSP como dos bordes dirigidos que apuntan en direcciones opuestas. Cada uno de estos bordes dirigidos se llama **arco**. El algoritmo de consistencia de arco funciona de la siguiente manera:

- Comience almacenando todos los arcos en el grafo de restricciones para el CSP en una cola $$Q$$.
- Elimine iterativamente los arcos de $$Q$$ y aplique la condición de que en cada arco eliminado $$X_i \longrightarrow X_j$$, para cada valor restante $$v$$ para la variable de cola $$X_i$$, hay al menos un valor restante $$w$$ para la variable de cabeza $$X_j$$ tal que $$X_i = v$$, $$X_j = w$$ no viola ninguna restricción. Si algún valor $$v$$ para $$X_i$$ no funcionara con ninguno de los valores restantes para $$X_j$$, eliminamos $$v$$ del conjunto de valores posibles para $$X_i$$.
- Si se elimina al menos un valor para $$X_i$$ al imponer la consistencia de arco para un arco $$X_i \longrightarrow X_j$$, agregue arcos de la forma $$X_k \longrightarrow X_i$$ a $$Q$$, para todas las variables no asignadas $$X_k$$. Si un arco $$X_k \longrightarrow X_i$$ ya está en $$Q$$ durante este paso, no es necesario agregarlo nuevamente.
- Continúe hasta que $$Q$$ esté vacío, o el dominio de alguna variable esté vacío y desencadene un retroceso.

El algoritmo de consistencia de arco generalmente no es el más intuitivo, así que veamos un ejemplo rápido con el coloreado de mapas:

<img src="{{ site.baseurl }}/assets/images/arc-consistency-ex-1.png" alt="Ejemplo de consistencia de arco" />

Comenzamos agregando todos los arcos entre variables no asignadas que comparten una restricción a una cola $$ Q $$, lo que nos da:

$$Q = [
    SA \rightarrow V, V \rightarrow SA, SA \rightarrow NSW, NSW \rightarrow SA, SA \rightarrow NT, NT \rightarrow SA, V \rightarrow NSW, NSW \rightarrow V]$$

Para nuestro primer arco, $$SA \rightarrow V$$, vemos que para cada valor en el dominio de $$SA$$, $$\{azul\}$$, hay *al menos* un valor en el dominio de $$V$$, $$\{rojo, verde, azul\}$$, que no viola ninguna restricción, por lo que no es necesario podar valores del dominio de $$SA$$. Sin embargo, para nuestro siguiente arco $$V \rightarrow SA$$, si establecemos $$V = \text{azul}$$ vemos que $$ SA $$ no tendrá valores restantes que no violen ninguna restricción, por lo que podamos $$\text{azul}$$ del dominio de $$V$$.

<img src="{{ site.baseurl }}/assets/images/arc-consistency-ex-2.png" alt="Ejemplo de consistencia de arco 2" />

Debido a que podamos un valor del dominio de $$V$$, necesitamos poner en cola todos los arcos con $$V$$ en la cabeza: $$SA \rightarrow V$$, $$NSW \rightarrow V$$. Dado que $$NSW \rightarrow V$$ ya está en $$Q$$, solo necesitamos agregar $$SA \rightarrow V$$, dejándonos con nuestra cola actualizada:

$$Q = [SA \rightarrow NSW, NSW \rightarrow SA, SA \rightarrow NT, NT \rightarrow SA, V \rightarrow NSW, NSW \rightarrow V, SA \rightarrow V]$$

Podemos continuar este proceso hasta que finalmente eliminemos el arco $$SA \rightarrow NT$$ de $$Q$$. Imponer la consistencia de arco en este arco elimina $$\text{azul}$$ del dominio de $$SA$$, dejándolo vacío y provocando un retroceso. Tenga en cuenta que el arco $$NSW \rightarrow SA$$ aparece antes de $$SA \rightarrow NT$$ en $$Q$$ y que imponer la consistencia en este arco elimina $$\text{azul}$$ del dominio de $$NSW$$.

<img src="{{ site.baseurl }}/assets/images/arc-consistency-ex-3.png" alt="Ejemplo de consistencia de arco 3" />

La consistencia de arco generalmente se implementa con el algoritmo AC-3 (Algoritmo de Consistencia de Arco #3), para el cual el pseudocódigo es el siguiente:

<img src="{{ site.baseurl }}/assets/images/arc-consistency-pseudo.png" alt="Pseudocódigo AC-3" />

El algoritmo AC-3 tiene una complejidad temporal en el peor de los casos de $$O(ed^3)$$, donde $$e$$ es el número de arcos (bordes dirigidos) y $$d$$ es el tamaño del dominio más grande. En general, la consistencia de arco es una técnica de poda de dominio más holística que la verificación hacia adelante y conduce a menos retrocesos, pero requiere ejecutar una cantidad significativamente mayor de cálculos para imponerla. En consecuencia, es importante tener en cuenta esta compensación al decidir qué técnica de filtrado implementar para el CSP que está intentando resolver.

Como nota final interesante sobre la consistencia, la consistencia de arco es un subconjunto de una noción más generalizada de consistencia conocida como **k-consistencia**, que cuando se impone garantiza que para cualquier conjunto de $$k$$ nodos en el CSP, una asignación consistente a cualquier subconjunto de $$k-1$$ nodos garantiza que el $$k$$-ésimo nodo tendrá al menos un valor consistente. Esta idea se puede extender aún más a través de la idea de **k-consistencia fuerte**. Un grafo que es fuerte $$k$$-consistente posee la propiedad de que cualquier subconjunto de $$k$$ nodos no solo es $$k$$-consistente sino también $$k-1, k-2, \dots, 1$$-consistente. No es sorprendente que imponer un mayor grado de consistencia en un CSP sea más costoso de calcular. Bajo esta definición generalizada de consistencia, podemos ver que la consistencia de arco es equivalente a la $$2$$-consistencia.
