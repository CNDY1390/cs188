---
title: 5.5 Resumen
parent: 5. RL
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 5.5 Resumen

Es muy importante recordar que el aprendizaje por refuerzo tiene un MDP subyacente, y el objetivo del aprendizaje por refuerzo es resolver este MDP derivando una política óptima. La diferencia entre usar el aprendizaje por refuerzo y usar métodos como la iteración de valor y la iteración de políticas es la falta de conocimiento de la función de transición $$T$$ y la función de recompensa $$R$$ para el MDP subyacente. Como resultado, los agentes deben *aprender* la política óptima a través de prueba y error en línea en lugar de puro cálculo fuera de línea. Hay muchas formas de hacer esto:

- **Aprendizaje basado en modelos**: Ejecuta cálculos para estimar los valores de la función de transición $$T$$ y la función de recompensa $$R$$ y utiliza métodos de resolución de MDP como la iteración de valor o política con estas estimaciones.
  
- **Aprendizaje libre de modelos**: Evita la estimación de $$T$$ y $$R$$, utilizando en su lugar otros métodos para estimar directamente los valores o valores Q de los estados.
  
  - **Evaluación directa**: Sigue una política $$\pi$$ y simplemente cuenta las recompensas totales obtenidas de cada estado y el número total de veces que se visita cada estado. Si se toman suficientes muestras, esto converge a los valores verdaderos de los estados bajo $$\pi$$, aunque es lento y desperdicia información sobre las transiciones entre estados.
  
  - **Aprendizaje por diferencia temporal**: Sigue una política $$\pi$$ y utiliza un promedio móvil exponencial con valores muestreados hasta la convergencia a los valores verdaderos de los estados bajo $$\pi$$. El aprendizaje TD y la evaluación directa son ejemplos de aprendizaje en política, que aprenden los valores para una política específica antes de decidir si esa política es subóptima y necesita actualizarse.
  
  - **Q-Learning**: Aprende la política óptima directamente a través de prueba y error con actualizaciones de iteración de valor Q. Este es un ejemplo de aprendizaje fuera de política, que aprende una política óptima incluso cuando se toman acciones subóptimas.
  
  - **Q-Learning aproximado**: Hace lo mismo que Q-learning pero utiliza una representación basada en características para los estados para generalizar el aprendizaje.

- Para cuantificar el rendimiento de diferentes algoritmos de aprendizaje por refuerzo, utilizamos la noción de **arrepentimiento**. El arrepentimiento captura la diferencia entre la recompensa total acumulada si actuáramos de manera óptima en el entorno desde el principio y la recompensa total acumulada al ejecutar el algoritmo de aprendizaje.
