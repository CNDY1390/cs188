---
title: 1.6 本章小结
parent: 1. 搜索
nav_order: 6
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 1.6 Summary (本章小结)

我们讨论了 *search problems*（搜索问题）及其组成部分：*state space*（状态空间）、一组 *actions*（动作）、*transition function*（转换函数）、*action cost*（动作成本）、*start state*（开始状态）和 *goal state*（目标状态）。智能体通过其传感器和执行器与环境交互。智能体函数描述了智能体在所有情况下的行为。智能体的理性意味着智能体寻求最大化其预期效用。最后，我们使用 PEAS 描述定义了我们的任务环境。

关于搜索问题，可以使用各种搜索技术来解决，包括但不限于我们在 CS 188 中学习的五种：

- *Breadth-first Search* (广度优先搜索)
- *Depth-first Search* (深度优先搜索)
- *Uniform Cost Search* (一致代价搜索)
- *Greedy Search* (贪婪搜索)
- *A* Search* (A* 搜索)

上面列出的前三种搜索技术是 *uninformed search*（无信息搜索）的例子，而后两种是 *informed search*（有信息搜索）的例子，它们使用 *heuristics*（启发式）来估计目标距离并优化性能。

我们还对上述技术的 *tree search*（树搜索）和 *graph search*（图搜索）算法进行了区分。

我们还讨论了 *local search algorithms*（局部搜索算法）及其动机。当我们不关心通往某个目标状态的路径并希望满足约束或优化目标时，我们可以使用这些方法。局部搜索方法允许我们在处理大型状态空间时节省空间并找到足够的解决方案！

我们回顾了几种基础的局部搜索方法，它们相互建立：

- *Hill-Climbing* (爬山法)
- *Simulated Annealing* (模拟退火)
- *Local Beam Search* (局部束搜索)
- *Genetic Algorithms* (遗传算法)

优化函数的想法将在本课程的后面再次出现，特别是当我们涵盖神经网络时。
