---
title: 3.5 Búsqueda de árbol Monte Carlo
parent: 3. Juegos
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 3.5 Búsqueda de árbol Monte Carlo

Para aplicaciones con un gran factor de ramificación, como jugar Go, ya no se puede utilizar minimax. Para tales aplicaciones, utilizamos el algoritmo de **Búsqueda de árbol Monte Carlo (MCTS)**. MCTS se basa en dos ideas:

- **Evaluación por despliegues (rollouts)**: Desde el estado $$s$$ jugar muchas veces usando una política (por ejemplo, aleatoria) y contar victorias/derrotas.
- **Búsqueda selectiva**: Explorar partes del árbol, sin restricciones en el horizonte, que mejorarán la decisión en la raíz.

En el ejemplo de Go, desde un estado dado, jugamos hasta la terminación de acuerdo con una política varias veces. Registramos la fracción de victorias, que se correlaciona bien con el valor del estado.

Considere el siguiente ejemplo:

<img src="{{ site.baseurl }}/assets/images/MCTS1.png" alt="Ejemplo MCTS 1" />

Desde el estado actual tenemos tres acciones disponibles diferentes (izquierda, centro y derecha). Tomamos cada acción $$100$$ veces y registramos el porcentaje de victorias para cada una. Después de las simulaciones, estamos bastante seguros de que la acción correcta es la mejor. En este escenario, asignamos la misma cantidad de simulaciones a cada acción alternativa. Sin embargo, podría quedar claro después de algunas simulaciones que una determinada acción no devuelve muchas victorias y, por lo tanto, podríamos optar por asignar este esfuerzo computacional para realizar más simulaciones para las otras acciones. Este caso se puede ver en la siguiente figura, donde decidimos asignar las $$90$$ simulaciones restantes para la acción central a las acciones izquierda y derecha.

<img src="{{ site.baseurl }}/assets/images/MCTS2.png" alt="Ejemplo MCTS 2" />

Surge un caso interesante cuando algunas acciones arrojan porcentajes similares de victorias, pero una de ellas ha utilizado muchas menos simulaciones para estimar ese porcentaje, como se muestra en la siguiente figura. En este caso, la estimación de la acción que utilizó menos simulaciones tendrá una varianza mayor y, por lo tanto, es posible que deseemos asignar algunas simulaciones más a esa acción para tener más confianza sobre el porcentaje real de victorias.

<img src="{{ site.baseurl }}/assets/images/MCTS3.png" alt="Ejemplo MCTS 3" />

El **algoritmo UCB** captura esta compensación entre acciones "prometedoras" e "inciertas" utilizando el siguiente criterio en cada nodo $$n$$:

$$UCB1(n)=\frac{U(n)}{N(n)}+C\times\sqrt{\frac{\log N(PARENT(n))}{N(n)}}$$

donde $$N(n)$$ denota el número total de despliegues desde el nodo $$n$$ y $$U(n)$$ el número total de victorias para $$Player(Parent(n))$$. El primer término captura cuán prometedor es el nodo, mientras que el segundo captura cuán inseguros estamos sobre la utilidad de ese nodo. El parámetro $$C$$ especificado por el usuario equilibra el peso que ponemos en los dos términos ("exploración" y "explotación") y depende de la aplicación y quizás de la etapa de la tarea (en etapas posteriores, cuando hayamos acumulado muchas pruebas, probablemente exploraríamos menos y explotaríamos más).

El algoritmo **MCTS UCT** utiliza el criterio UCB en problemas de búsqueda de árboles. Más específicamente, repite los siguientes tres pasos varias veces:

1. El criterio UCB se utiliza para descender por las capas de un árbol desde el nodo raíz hasta que se alcanza un nodo hoja no expandido.
2. Se agrega un nuevo hijo a esa hoja y ejecutamos un despliegue desde ese hijo para determinar el número de victorias desde ese nodo.
3. Actualizamos el número de victorias desde el hijo hasta el nodo raíz.

Una vez que los tres pasos anteriores se repiten lo suficiente, elegimos la acción que conduce al hijo con el $$N$$ más alto. Tenga en cuenta que debido a que UCT explora inherentemente a los niños más prometedores un mayor número de veces, a medida que $$N \rightarrow \infty$$, UCT se acerca al comportamiento de un agente minimax.
