---
title: 6.8 总结
parent: 6. 贝叶斯网络
nav_order: 8
layout: page
header-includes:
    \pagenumbering{gobble}
---

## 6.8 总结 (Summary)

总而言之，贝叶斯网络是联合概率分布的强大表示。它的拓扑结构编码了独立和条件独立关系，我们可以使用它来建模任意分布以执行推理和采样。

在本笔记中，我们介绍了两种概率推理方法：精确推理和概率推理（采样）。在精确推理中，我们保证得到精确正确的概率，但计算量可能令人望而却步。

涵盖的精确推理算法有：
- 枚举推理 (Inference By Enumeration)
- 变量消元 (Variable Elimination)

我们可以转向采样来近似解，同时使用更少的计算。

涵盖的采样算法有：
- 先验采样 (Prior Sampling)
- 拒绝采样 (Rejection Sampling)
- 似然加权 (Likelihood Weighting)
- 吉布斯采样 (Gibbs Sampling)
