---
title: 9.1 机器学习
parent: 9. 机器学习
nav_order: 1
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 9.1 机器学习 (Machine Learning)

在本课程的前几篇笔记中，我们了解了各种类型的模型，这些模型帮助我们在不确定性下进行推理。直到现在，我们一直假设我们使用的概率模型是理所当然的，并且生成我们使用的底层概率表的方法已被抽象化。当我们深入讨论**机器学习 (machine learning)** 时，我们将开始打破这种抽象障碍，这是计算机科学的一个广泛领域，涉及在给定一些数据的情况下构建和/或学习指定模型的参数。

有许多机器学习算法处理许多不同类型的问题和不同类型的数据，根据它们希望完成的任务和它们处理的数据类型进行分类。机器学习算法的两个主要子组是**监督学习算法 (supervised learning algorithms)** 和**无监督学习算法 (unsupervised learning algorithms)**。监督学习算法推断输入数据与相应输出数据之间的关系，以便预测新的、以前未见过的输入数据的输出。另一方面，无监督学习算法的输入数据没有任何相应的输出数据，因此处理识别数据点之间或内部的固有结构并相应地对它们进行分组和/或处理。在本课程中，我们将讨论的算法将仅限于监督学习任务。

<div style="display: flex; justify-content: space-around; align-items: center;">
  <div style="text-align: center;">
    <img src="{{ site.baseurl }}/assets/images/train.png" alt="Image 1" style="width: 200px; height: 150px; object-fit: cover;">
    <div><b>(a) 训练 (Training)</b></div>
  </div>
  <div style="text-align: center;">
    <img src="{{ site.baseurl }}/assets/images/validation.png" alt="Image 2" style="width: 200px; height: 150px; object-fit: cover;">
    <div><b>(b) 验证 (Validation)</b></div>
  </div>
  <div style="text-align: center;">
    <img src="{{ site.baseurl }}/assets/images/test.png" alt="Image 3" style="width: 200px; height: 150px; object-fit: cover;">
    <div><b>(c) 测试 (Testing)</b></div>
  </div>
</div>

一旦你有了准备好用来学习的数据集，机器学习过程通常涉及将你的数据集分成三个不同的子集。第一个是**训练数据 (training data)**，用于实际生成将输入映射到输出的模型。然后，**验证数据 (validation data)**（也称为**留出 (hold-out)** 或**开发数据 (development data)**）用于通过对输入进行预测并生成准确度分数来衡量模型的性能。如果你的模型表现不如你所愿，总是可以回去重新训练，或者通过调整称为**超参数 (hyperparameters)** 的特殊模型特定值，或者通过使用完全不同的学习算法，直到你对结果满意为止。最后，使用你的模型对数据的第三个也是最后一个子集，即**测试集 (test set)** 进行预测。测试集是你的代理直到开发结束才看到的数据部分，相当于衡量现实世界数据性能的“期末考试”。

在接下来的内容中，我们将介绍一些基础的机器学习算法，例如朴素贝叶斯、线性回归、逻辑回归和感知器算法。
