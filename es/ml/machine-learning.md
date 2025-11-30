---
title: "9.1 Aprendizaje Automático"
parent: 9. Aprendizaje Automático
nav_order: 1
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 9.1 Aprendizaje Automático

En las notas anteriores de este curso, hemos aprendido sobre varios tipos de modelos que nos ayudan a razonar bajo incertidumbre. Hasta ahora, hemos asumido que los modelos probabilísticos con los que hemos trabajado se pueden dar por sentados, y los métodos mediante los cuales se generaron las tablas de probabilidad subyacentes con las que trabajamos se han abstraído. Comenzaremos a romper esta barrera de abstracción a medida que profundizamos en nuestra discusión sobre el **aprendizaje automático** (*machine learning*), un amplio campo de la informática que se ocupa de construir y/o aprender los parámetros de un modelo específico a partir de algunos datos.

Existen muchos algoritmos de aprendizaje automático que tratan con muchos tipos diferentes de problemas y diferentes tipos de datos, clasificados según las tareas que esperan lograr y los tipos de datos con los que trabajan. Dos subgrupos principales de algoritmos de aprendizaje automático son los **algoritmos de aprendizaje supervisado** y los **algoritmos de aprendizaje no supervisado**. Los algoritmos de aprendizaje supervisado infieren una relación entre los datos de entrada y los datos de salida correspondientes para predecir salidas para nuevos datos de entrada no vistos anteriormente. Los algoritmos de aprendizaje no supervisado, por otro lado, tienen datos de entrada que no tienen ningún dato de salida correspondiente y, por lo tanto, tratan de reconocer la estructura inherente entre o dentro de los puntos de datos y agruparlos y/o procesarlos en consecuencia. En esta clase, los algoritmos que discutiremos se limitarán a tareas de aprendizaje supervisado.

<div style="display: flex; justify-content: space-around; align-items: center;">
  <div style="text-align: center;">
    <img src="{{ site.baseurl }}/assets/images/train.png" alt="Imagen 1" style="width: 200px; height: 150px; object-fit: cover;">
    <div><b>(a) Entrenamiento</b></div>
  </div>
  <div style="text-align: center;">
    <img src="{{ site.baseurl }}/assets/images/validation.png" alt="Imagen 2" style="width: 200px; height: 150px; object-fit: cover;">
    <div><b>(b) Validación</b></div>
  </div>
  <div style="text-align: center;">
    <img src="{{ site.baseurl }}/assets/images/test.png" alt="Imagen 3" style="width: 200px; height: 150px; object-fit: cover;">
    <div><b>(c) Prueba</b></div>
  </div>
</div>

Una vez que tienes un conjunto de datos con el que estás listo para aprender, el proceso de aprendizaje automático generalmente implica dividir tu conjunto de datos en tres subconjuntos distintos. El primero, **datos de entrenamiento**, se utiliza para generar realmente un modelo que mapea entradas a salidas. Luego, los **datos de validación** (también conocidos como **datos de retención** o **datos de desarrollo**) se utilizan para medir el rendimiento de tu modelo haciendo predicciones sobre entradas y generando una puntuación de precisión. Si tu modelo no funciona tan bien como te gustaría, siempre está bien volver atrás y entrenar de nuevo, ya sea ajustando valores especiales específicos del modelo llamados **hiperparámetros** o utilizando un algoritmo de aprendizaje diferente por completo hasta que estés satisfecho con tus resultados. Finalmente, usa tu modelo para hacer predicciones en el tercer y último subconjunto de tus datos, el **conjunto de prueba**. El conjunto de prueba es la parte de tus datos que nunca es vista por tu agente hasta el final del desarrollo, y es el equivalente a un "examen final" para medir el rendimiento en datos del mundo real.

En lo que sigue, cubriremos algunos algoritmos fundamentales de aprendizaje automático, como Naive Bayes, Regresión Lineal, Regresión Logística y el algoritmo Perceptrón.
