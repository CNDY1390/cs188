---
title: 5.4 Exploración y Explotación
parent: 5. RL
nav_order: 4
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 5.4 Exploración y Explotación

Ahora hemos cubierto varios métodos diferentes para que un agente aprenda una política óptima, y enfatizamos que la "exploración suficiente" es necesaria para esto, sin elaborar realmente sobre lo que significa "suficiente". En las siguientes secciones, discutiremos dos métodos para distribuir el tiempo entre la exploración y la explotación: políticas $$\epsilon$$-codiciosas y funciones de exploración.

## 5.4.1 Políticas $$\varepsilon$$-codiciosas

Los agentes que siguen una **política $$\epsilon$$-codiciosa** definen alguna probabilidad $$0 \leq \epsilon \leq 1$$, y actúan aleatoriamente y exploran con probabilidad $$\epsilon$$. En consecuencia, siguen su política establecida actual y explotan con probabilidad $$(1 - \epsilon)$$. Esta es una política muy simple de implementar, pero aún puede ser bastante difícil de manejar. Si se selecciona un valor grande para $$\epsilon$$, incluso después de aprender la política óptima, el agente seguirá comportándose principalmente de forma aleatoria. De manera similar, seleccionar un valor pequeño para $$\epsilon$$ significa que el agente explorará con poca frecuencia, lo que llevará a Q-learning (o cualquier otro algoritmo de aprendizaje seleccionado) a aprender la política óptima muy lentamente. Para evitar esto, $$\epsilon$$ debe ajustarse manualmente y reducirse con el tiempo para ver resultados.

## 5.4.2 Funciones de exploración

El problema de ajustar manualmente $$\epsilon$$ se evita mediante **funciones de exploración**, que utilizan una actualización de iteración de valor Q modificada para dar cierta preferencia a visitar estados menos visitados. La actualización modificada es la siguiente:

$$Q(s, a) \leftarrow (1-\alpha)Q(s, a) + \alpha \cdot [R(s, a, s') + \gamma \max_{a'} f(s', a')]$$

donde $$f$$ denota una función de exploración. Existe cierto grado de flexibilidad en el diseño de una función de exploración, pero una opción común es usar:

$$f(s, a) = Q(s, a) + \frac{k}{N(s, a)}$$

con $$k$$ siendo algún valor predeterminado, y $$N(s, a)$$ denotando el número de veces que se ha visitado el estado Q $$(s, a)$$. Los agentes en un estado $$s$$ siempre seleccionan la acción que tiene el $$f(s, a)$$ más alto de cada estado y, por lo tanto, nunca tienen que tomar una decisión probabilística entre exploración y explotación. En cambio, la exploración se codifica automáticamente mediante la función de exploración, ya que el término $$\frac{k}{N(s, a)}$$ puede dar suficiente "bonificación" a alguna acción tomada con poca frecuencia de modo que se seleccione sobre acciones con valores Q más altos. A medida que pasa el tiempo y los estados se visitan con mayor frecuencia, esta bonificación disminuye hacia $$0$$ para cada estado y $$f(s, a)$$ regresa hacia $$Q(s, a)$$, haciendo que la explotación sea cada vez más exclusiva.
