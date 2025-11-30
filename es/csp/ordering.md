---
title: 2.4 Ordenamiento
parent: 2. CSPs
nav_order: 4
layout: page
lang: es
header-includes:
    \pagenumbering{gobble}
---

# 2.4 Ordenamiento

Hemos delineado que al resolver un CSP, fijamos algún orden tanto para las variables como para los valores involucrados. En la práctica, a menudo es mucho más efectivo calcular la siguiente variable y el valor correspondiente "sobre la marcha" con dos principios generales, **valores restantes mínimos** y **valor menos restrictivo**:

- *Valores restantes mínimos (MRV)* - Al seleccionar qué variable asignar a continuación, el uso de una política MRV elige la variable no asignada que tenga la menor cantidad de valores restantes válidos (la *variable más restringida*). Esto es intuitivo en el sentido de que es más probable que la variable más restringida se quede sin valores posibles y resulte en un retroceso si se deja sin asignar, por lo que es mejor asignarle un valor más temprano que tarde.
- *Valor menos restrictivo (LCV)* - De manera similar, al seleccionar qué valor asignar a continuación, una buena política para implementar es seleccionar el valor que poda la menor cantidad de valores de los dominios de los valores no asignados restantes. En particular, esto requiere un cálculo adicional (por ejemplo, volver a ejecutar la consistencia de arco/verificación hacia adelante u otros métodos de filtrado para cada valor para encontrar el LCV), pero aún puede generar ganancias de velocidad según el uso.

## 2.4.1 Estructura

Una clase final de mejoras para resolver problemas de satisfacción de restricciones son aquellas que explotan su estructura. En particular, si estamos tratando de resolver un **CSP estructurado en árbol** (uno que no tiene bucles en su grafo de restricciones), podemos reducir el tiempo de ejecución para encontrar una solución de $$O(d^N)$$ hasta $$O(nd^2)$$, lineal en el número de variables. Esto se puede hacer con el algoritmo CSP estructurado en árbol, que se describe a continuación:

- Primero, elija un nodo arbitrario en el grafo de restricciones para que el CSP sirva como la raíz del árbol (no importa cuál porque la teoría de grafos básica nos dice que cualquier nodo de un árbol puede servir como raíz).
- Convierta todos los bordes no dirigidos del árbol en bordes dirigidos que apunten *lejos* de la raíz. Luego **linealice** (o **ordene topológicamente**) el grafo acíclico dirigido resultante. En términos simples, esto solo significa ordenar los nodos del grafo de tal manera que todos los bordes apunten hacia la derecha. Teniendo en cuenta que seleccionamos el nodo $$A$$ para que sea nuestra raíz y dirigimos todos los bordes para que apunten lejos de $$A$$, este proceso da como resultado la siguiente conversión para el CSP presentado a continuación:

<img src="{{ site.baseurl }}/assets/images/tree-structured-alg.png" alt="CSP estructurado en árbol" />

- Realice un **paso hacia atrás** de consistencia de arco. Iterando desde $$i = n$$ hasta $$i = 2$$, imponga la consistencia de arco para todos los arcos $$Padre(X_i) \longrightarrow X_i$$. Para el CSP linealizado de arriba, esta poda de dominio eliminará algunos valores, dejándonos con lo siguiente:

<img src="{{ site.baseurl }}/assets/images/pruned-tree.png" alt="Árbol podado" />

- Finalmente, realice una **asignación hacia adelante**. Comenzando desde $$X_1$$ y yendo a $$X_n$$, asigne a cada $$X_i$$ un valor consistente con el de su padre. Debido a que hemos impuesto la consistencia de arco en todos estos arcos, no importa qué valor seleccionemos para cualquier nodo, sabemos que sus hijos tendrán cada uno al menos un valor consistente. Por lo tanto, esta asignación iterativa garantiza una solución correcta, un hecho que se puede probar inductivamente sin dificultad.

El algoritmo estructurado en árbol se puede extender a CSP que están razonablemente cerca de estar estructurados en árbol con **condicionamiento de conjunto de corte**. El condicionamiento de conjunto de corte implica primero encontrar el subconjunto más pequeño de variables en un grafo de restricciones tal que su eliminación resulte en un árbol (dicho subconjunto se conoce como **conjunto de corte** para el grafo). Por ejemplo, en nuestro ejemplo de coloreado de mapas, Australia del Sur ($$SA$$) es el conjunto de corte más pequeño posible:

<img src="{{ site.baseurl }}/assets/images/cutset.png" alt="Ejemplo de conjunto de corte" />

Una vez que se encuentra el conjunto de corte más pequeño, asignamos todas las variables en él y podamos los dominios de todos los nodos vecinos. ¡Lo que queda es un CSP estructurado en árbol, sobre el cual podemos resolver con el algoritmo CSP estructurado en árbol de arriba! La asignación inicial a un conjunto de corte de tamaño $$c$$ puede dejar el CSP estructurado en árbol resultante sin solución válida después de la poda, por lo que es posible que aún necesitemos retroceder hasta $$d^c$$ veces. Dado que la eliminación del conjunto de corte nos deja con un CSP estructurado en árbol con $$(n - c)$$ variables, sabemos que esto se puede resolver (o determinar que no existe solución) en $$O((n - c)d^2)$$. Por lo tanto, el tiempo de ejecución del condicionamiento de conjunto de corte en un CSP general es $$O(d^c(n-c)d^2)$$, muy bueno para $$c$$ pequeño.
