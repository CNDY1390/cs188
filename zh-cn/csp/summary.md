---
title: 2.6 总结
parent: 2. CSPs
nav_order: 6
layout: page
 
header-includes:
    \pagenumbering{gobble}
---

# 2.6 总结 (Summary)

重要的是要记住，通常情况下，约束满足问题没有一个相对于变量数量为多项式时间的高效算法。然而，通过使用各种启发式方法，我们通常可以在可接受的时间内找到解：

-   *过滤 (Filtering)* - 过滤处理提前修剪未赋值变量的定义域，以防止不必要的回溯。我们介绍的两种重要的过滤技术是*前向检查 (forward checking)* 和 *弧一致性 (arc consistency)*。
-   *排序 (Ordering)* - 排序处理选择下一个要赋值的变量或值，以尽可能减少回溯的可能性。对于变量选择，我们学习了 *MRV 策略*，对于值选择，我们学习了 *LCV 策略*。
-   *结构 (Structure)* - 如果 CSP 是树状结构的或接近树状结构的，我们可以对其运行树状结构 CSP 算法，以线性时间得出解。同样，如果 CSP 接近树状结构，我们可以使用*割集调节 (cutset conditioning)* 将 CSP 转换为一个或多个独立的树状结构 CSP，并分别解决它们。
