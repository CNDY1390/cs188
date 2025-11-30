---
title: 3.4 Juegos generales
parent: 3. Juegos
nav_order: 4
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 3.4 Juegos generales

No todos los juegos son de suma cero. De hecho, diferentes agentes pueden tener tareas distintas en un juego que no implican directamente competir estrictamente entre sí. Tales juegos se pueden configurar con árboles caracterizados por **utilidades multiagente**. Tales utilidades, en lugar de ser un valor único que los agentes alternos intentan minimizar o maximizar, se representan como tuplas con diferentes valores dentro de la tupla correspondientes a utilidades únicas para diferentes agentes. Luego, cada agente intenta maximizar su propia utilidad en cada nodo que controla, ignorando las utilidades de otros agentes. Considere el siguiente árbol:

<img src="{{ site.baseurl }}/assets/images/multi-agent-utility.png" alt="Utilidad multiagente" />

Los nodos rojo, verde y azul corresponden a tres agentes separados, que maximizan las utilidades roja, verde y azul respectivamente de las opciones posibles en sus respectivas capas. Trabajar a través de este ejemplo finalmente produce la tupla de utilidad $(5, 2, 5)$ en la parte superior del árbol. Los juegos generales con utilidades multiagente son un excelente ejemplo del surgimiento del comportamiento a través de la computación, ya que tales configuraciones invocan la cooperación ya que la utilidad seleccionada en la raíz del árbol tiende a producir una utilidad razonable para todos los agentes participantes.
