---
title: 2.2 解决约束满足问题
parent: 2. CSPs
nav_order: 2
layout: page
lang: zh-cn
header-includes:
    \pagenumbering{gobble}
---

# 2.2 解决约束满足问题 (Solving Constraint Satisfaction Problems)

约束满足问题传统上使用一种称为**回溯搜索 (backtracking search)** 的搜索算法来解决。回溯搜索是深度优先搜索的一种优化，专门用于解决约束满足问题，其改进主要来自两个原则：

1.  固定变量的顺序，并按此顺序为变量选择值。因为赋值是可交换的（例如，赋值 $$WA = \text{Red},\:\: NT = \text{Green}$$ 等同于 $$NT = \text{Green},\:\: WA = \text{Red}$$），所以这是有效的。
2.  在为变量选择值时，只选择与任何先前赋值的值不冲突的值。如果不存在这样的值，则回溯并返回到上一个变量，更改其值。

递归回溯的工作原理伪代码如下：

<img src="{{ site.baseurl }}/assets/images/backtracking-search-pseudo.png" alt="Backtracking search pseudocode" />

为了可视化这个过程是如何工作的，考虑地图着色中深度优先搜索和回溯搜索的部分搜索树：

<img src="{{ site.baseurl }}/assets/images/dfs-vs-backtracking.png" alt="DFS vs Backtracking search" />

注意 DFS 在意识到需要改变之前是如何遗憾地将所有东西都涂成红色的，即使那样，也没有朝着解的正确方向移动太远。另一方面，回溯搜索只有在值不违反任何约束时才将其赋值给变量，从而导致回溯显著减少。虽然回溯搜索比深度优先搜索的暴力破解有了巨大的改进，但我们仍然可以通过过滤、变量/值排序和结构利用等进一步改进来获得更高的速度提升。
