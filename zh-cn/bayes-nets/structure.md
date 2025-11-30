---
title: '6.4 贝叶斯网络的结构'
parent: 6. 贝叶斯网络
nav_order: 4
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.4 贝叶斯网络的结构 (Structure of Bayes Nets)

在这门课中，我们将参考贝叶斯网络独立性的两个规则，这些规则可以通过查看贝叶斯网络的图形结构来推断：

- **在给定所有父节点的情况下，每个节点条件独立于图中的所有祖先节点（非后代）。**

<img src="{{ site.baseurl }}/assets/images/parents.png" alt="Parents" />

- **在给定马尔可夫毯 (Markov blanket) 的情况下，每个节点条件独立于所有其他变量。** 一个变量的马尔可夫毯由父节点、子节点和子节点的其他父节点组成。

<img src="{{ site.baseurl }}/assets/images/blanket.png" alt="Markov Blanket" />

使用这些工具，我们可以回到上一节中的断言：我们可以通过连接贝叶斯网络的 CPT 来获得所有变量的联合分布。

$$P(X_1, X_2, \dots, X_n) = \prod_{i=1}^n P(X_i | \text{parents}(X_i))$$

联合分布和贝叶斯网络的 CPT 之间的这种关系之所以有效，是因为图给出的条件独立关系。我们将使用一个例子来证明这一点。
<p></p>
让我们回顾一下前面的例子。我们有 CPT $$P(B)$$ , $$P(E)$$ , $$P(A |B,E)$$ , $$P(J | A)$$ 和 $$P(M | A)$$ ，以及下图：

<img src="{{ site.baseurl }}/assets/images/basic_bayes_nets.png" alt="Basic Bayes Net Examples" />

对于这个贝叶斯网络，我们试图证明以下关系：

$$P(B, E, A, J, M) = P(B)P(E)P(A | B, E)P(J | A)P(M | A)$$

我们可以用另一种方式展开联合分布：使用链式法则。如果我们用拓扑排序（父节点在子节点之前）展开联合分布，我们得到以下方程：

$$P(B, E, A, J, M) = P(B)P(E | B)P(A | B, E)P(J | B, E, A)P(M | B, E, A, J)$$
<p></p>
注意，在第一个方程中，每个变量都表示在一个 CPT $$P(var | Parents(var))$$ 中，而在第二个方程中，每个变量都表示在一个 CPT $$P(var | Parents(var), Ancestors(var))$$ 中。

我们依赖于上面的第一个条件独立关系，即**在给定所有父节点的情况下，每个节点条件独立于图中的所有祖先节点**[^1]。
<p></p>
因此，在贝叶斯网络中，$$P(var | Parents(var), Ancestors(var)) = P(var | Parents(var))$$ ，所以这两个方程是相等的。贝叶斯网络中的条件独立性允许使用多个较小的条件概率表来表示整个联合概率分布。

[^1]: 在其他地方，该假设可能被定义为“给定父节点，节点条件独立于其_非后代_。”我们总是希望做出尽可能小的假设并证明我们需要什么，所以我们将使用祖先假设。
