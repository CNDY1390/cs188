---
title: 1.1 Agentes
parent: 1. Búsqueda
nav_order: 1
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 1.1 Agentes

En inteligencia artificial, el problema central es la creación de un **agente** racional, una entidad que tiene objetivos o preferencias e intenta realizar una serie de **acciones** que produzcan el mejor/óptimo resultado esperado dados estos objetivos. Los agentes racionales existen en un **entorno**, que es específico para la instanciación dada del agente. Los agentes utilizan sensores para interactuar con el entorno y actúan sobre él utilizando actuadores. Como ejemplo muy simple, el entorno para un agente de damas es el tablero de damas virtual en el que juega contra oponentes, donde los movimientos de las piezas son acciones. Juntos, un entorno y los agentes que residen en él crean un **mundo**.

Un **agente reflejo** es aquel que no piensa en las consecuencias de sus acciones, sino que selecciona una acción basándose únicamente en el estado actual del mundo. Estos agentes suelen ser superados por los **agentes de planificación**, que mantienen un modelo del mundo y utilizan este modelo para simular la realización de diversas acciones. Luego, el agente puede determinar las consecuencias hipotéticas de las acciones y seleccionar la mejor. Esto es "inteligencia" simulada en el sentido de que es exactamente lo que hacen los humanos cuando intentan determinar el mejor movimiento posible en cualquier situación: pensar con anticipación.

Para definir el entorno de la tarea, utilizamos la descripción **PEAS** (**P**erformance Measure, **E**nvironment, **A**ctuators, **S**ensors; Medida de rendimiento, Entorno, Actuadores, Sensores). La medida de rendimiento describe qué utilidad intenta aumentar el agente. El entorno resume dónde actúa el agente y qué afecta al agente. Los actuadores y los sensores son los métodos con los que el agente actúa sobre el entorno y recibe información de él.

El **diseño** de un agente depende en gran medida del tipo de entorno en el que actúa el agente. Podemos caracterizar los tipos de entornos de las siguientes maneras:

- En entornos *parcialmente observables*, el agente no tiene información completa sobre el estado y, por lo tanto, debe tener una estimación interna del estado del mundo. Esto contrasta con los entornos *totalmente observables*, donde el agente tiene información completa sobre su estado.
- Los entornos *estocásticos* tienen incertidumbre en el modelo de transición, es decir, realizar una acción en un estado específico puede tener múltiples resultados posibles con diferentes probabilidades. Esto contrasta con los entornos *deterministas*, donde realizar una acción en un estado tiene un único resultado que está garantizado que suceda.
- En entornos *multiagente*, el agente actúa junto con otros agentes. Por esta razón, el agente podría necesitar aleatorizar sus acciones para evitar ser "predecible" por otros agentes.
- Si el entorno no cambia a medida que el agente actúa sobre él, se llama *estático*. Esto contrasta con los entornos *dinámicos* que cambian a medida que el agente interactúa con ellos.
- Si un entorno tiene *física conocida*, entonces el modelo de transición (incluso si es estocástico) es conocido por el agente, y puede usarlo al planificar un camino. Si la *física es desconocida*, el agente necesitará realizar acciones deliberadamente para aprender la dinámica desconocida.
