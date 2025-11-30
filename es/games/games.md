---
title: 3.1 Juegos
parent: 3. Juegos
nav_order: 1
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 3.1 Juegos

En la primera nota, hablamos sobre los problemas de búsqueda y cómo resolverlos de manera eficiente y óptima: utilizando potentes algoritmos de búsqueda generalizados, nuestros agentes podían determinar el mejor plan posible y luego simplemente ejecutarlo para llegar a un objetivo. Ahora, cambiemos de marcha y consideremos escenarios en los que nuestros agentes tienen uno o más **adversarios** que intentan evitar que alcancen su(s) objetivo(s). Nuestros agentes ya no pueden ejecutar los algoritmos de búsqueda que ya hemos aprendido para formular un plan, ya que normalmente no sabemos de manera determinista cómo nuestros adversarios planearán contra nosotros y responderán a nuestras acciones. En cambio, necesitaremos ejecutar una nueva clase de algoritmos que produzcan soluciones a **problemas de búsqueda adversaria**, más comúnmente conocidos como **juegos**.

Hay muchos tipos diferentes de juegos. Los juegos pueden tener acciones con resultados deterministas o **estocásticos** (probabilísticos), pueden tener cualquier número variable de jugadores y pueden ser o no de **suma cero**. La primera clase de juegos que cubriremos son los **juegos deterministas de suma cero**, donde las acciones son deterministas y nuestra ganancia es directamente equivalente a la pérdida de nuestro oponente y viceversa. La forma más fácil de pensar en tales juegos es definirlos por un valor variable único, que un equipo o agente intenta maximizar y el equipo o agente contrario intenta minimizar, poniéndolos efectivamente en competencia directa. En Pacman, esta variable es tu puntuación, que intentas maximizar comiendo bolitas de forma rápida y eficiente, mientras que los fantasmas intentan minimizarla comiéndote primero. Muchos juegos domésticos comunes también caen bajo esta clase de juegos:

- *Damas* - El primer jugador de computadora de damas se creó en 1950. Desde entonces, las damas se han convertido en un **juego resuelto**, lo que significa que cualquier posición puede evaluarse como una victoria, derrota o empate de manera determinista para cualquier lado dado que ambos jugadores actúan de manera óptima.
- *Ajedrez* - En 1997, Deep Blue se convirtió en el primer agente informático en derrotar al campeón humano de ajedrez Garry Kasparov en un partido de seis juegos. Deep Blue fue construido para usar métodos extremadamente sofisticados para evaluar más de 200 millones de posiciones por segundo. Los programas actuales son aún mejores, aunque menos históricos.
- *Go* - El espacio de búsqueda para Go es mucho más grande que para el ajedrez, y la mayoría no creía que los agentes informáticos de Go derrotaran a los campeones mundiales humanos en varios años. Sin embargo, AlphaGo, desarrollado por Google, derrotó históricamente al campeón de Go Lee Sedol 4 juegos a 1 en marzo de 2016.

<img src="{{ site.baseurl }}/assets/images/common-games.png" alt="Juegos comunes" />

Todos los agentes campeones mundiales anteriores utilizan, al menos hasta cierto punto, las técnicas de búsqueda adversaria que estamos a punto de cubrir. A diferencia de la búsqueda normal, que devolvía un plan integral, la búsqueda adversaria devuelve una **estrategia**, o **política**, que simplemente recomienda el mejor movimiento posible dada alguna configuración de nuestro(s) agente(s) y sus adversarios. Pronto veremos que tales algoritmos tienen la hermosa propiedad de dar lugar al comportamiento a través de la computación: la computación que ejecutamos es relativamente simple en concepto y ampliamente generalizable, pero genera innatamente cooperación entre agentes del mismo equipo, así como "pensar mejor" que los agentes adversarios.

La formulación estándar del juego consta de las siguientes definiciones:

- Estado inicial, $$s_0$$
- Jugadores, $$Players(s)$$ denota de quién es el turno
- Acciones, $$Actions(s)$$ acciones disponibles para el jugador
- Modelo de transición $$Result(s, a)$$
- Prueba terminal, $$Terminal-test(s)$$
- Valores terminales, $$Utility(s, player)$$
