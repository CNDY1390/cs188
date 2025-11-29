---
title: 2.3 过滤
parent: 2. CSPs
nav_order: 3
layout: page
lang: zh-cn
header-includes:
    \pagenumbering{gobble}
---

# 2.3 过滤 (Filtering)

我们要考虑的第一个 CSP 性能改进是**过滤 (filtering)**，它检查我们是否可以通过移除那些我们知道会导致回溯的值，来提前修剪未赋值变量的定义域。一种朴素的过滤方法是**前向检查 (forward checking)**，每当一个值被赋值给一个变量 $$X_i$$ 时，它就会修剪那些与 $$X_i$$ 共享约束的未赋值变量的定义域，如果这些变量被赋值，将会违反约束。每当一个新的变量被赋值，我们就可以运行前向检查，并修剪约束图中与新赋值变量相邻的未赋值变量的定义域。考虑我们的地图着色示例，以及未赋值变量及其潜在值：

<img src="{{ site.baseurl }}/assets/images/forward-checking.png" alt="Forward checking example" />

注意，当我们赋值 $$WA = \text{red}$$ 然后 $$Q = \text{green}$$ 时，$$NT$$、$$NSW$$ 和 $$SA$$（与 $$WA$$、$$Q$$ 或两者都相邻的州）的定义域大小随着值的消除而减小。前向检查的思想可以推广为**弧一致性 (arc consistency)** 原则。对于弧一致性，我们将 CSP 约束图的每条无向边解释为两条指向相反方向的有向边。每一条这样的有向边被称为一个**弧 (arc)**。弧一致性算法的工作原理如下：

-   首先将 CSP 约束图中的所有弧存储在一个队列 $$Q$$ 中。
-   迭代地从 $$Q$$ 中移除弧，并强制执行以下条件：在每个移除的弧 $$X_i \longrightarrow X_j$$ 中，对于尾部变量 $$X_i$$ 的每个剩余值 $$v$$，头部变量 $$X_j$$ 至少有一个剩余值 $$w$$，使得 $$X_i = v$$，$$X_j = w$$ 不违反任何约束。如果 $$X_i$$ 的某个值 $$v$$ 无法与 $$X_j$$ 的任何剩余值一起工作，我们将 $$v$$ 从 $$X_i$$ 的可能值集合中移除。
-   如果在对弧 $$X_i \longrightarrow X_j$$ 强制执行弧一致性时，$$X_i$$ 至少有一个值被移除，则将形式为 $$X_k \longrightarrow X_i$$ 的弧添加到 $$Q$$ 中，其中 $$X_k$$ 为所有未赋值变量。如果弧 $$X_k \longrightarrow X_i$$ 在此步骤中已经在 $$Q$$ 中，则无需再次添加。
-   继续直到 $$Q$$ 为空，或者某个变量的定义域为空并触发回溯。

弧一致性算法通常不是最直观的，所以让我们通过一个快速的地图着色示例来演示：

<img src="{{ site.baseurl }}/assets/images/arc-consistency-ex-1.png" alt="Arc consistency example" />

我们首先将共享约束的未赋值变量之间的所有弧添加到一个队列 $$ Q $$ 中，这给我们：

$$Q = [
    SA \rightarrow V, V \rightarrow SA, SA \rightarrow NSW, NSW \rightarrow SA, SA \rightarrow NT, NT \rightarrow SA, V \rightarrow NSW, NSW \rightarrow V]$$

对于我们的第一个弧，$$SA \rightarrow V$$，我们看到对于 $$SA$$ 定义域中的每个值 $$\{blue\}$$，在 $$V$$ 的定义域 $$\{red, green, blue\}$$ 中*至少*有一个值不违反任何约束，因此不需要从 $$SA$$ 的定义域中修剪任何值。然而，对于我们的下一个弧 $$V \rightarrow SA$$，如果我们设置 $$V = \text{blue}$$，我们会看到 $$ SA $$ 将没有剩余的不违反约束的值，因此我们将 $$\text{blue}$$ 从 $$V$$ 的定义域中修剪掉。

<img src="{{ site.baseurl }}/assets/images/arc-consistency-ex-2.png" alt="Arc consistency example 2" />

因为我们从 $$V$$ 的定义域中修剪了一个值，我们需要将所有以 $$V$$ 为头部的弧入队 - $$SA \rightarrow V$$，$$NSW \rightarrow V$$。由于 $$NSW \rightarrow V$$ 已经在 $$Q$$ 中，我们只需要添加 $$SA \rightarrow V$$，留下我们更新后的队列：

$$Q = [SA \rightarrow NSW, NSW \rightarrow SA, SA \rightarrow NT, NT \rightarrow SA, V \rightarrow NSW, NSW \rightarrow V, SA \rightarrow V]$$

我们可以继续这个过程，直到我们最终从 $$Q$$ 中移除弧 $$SA \rightarrow NT$$。在这个弧上强制执行弧一致性会从 $$SA$$ 的定义域中移除 $$\text{blue}$$，使其为空并触发回溯。注意，弧 $$NSW \rightarrow SA$$ 在 $$Q$$ 中出现在 $$SA \rightarrow NT$$ 之前，并且在这个弧上强制执行一致性会从 $$NSW$$ 的定义域中移除 $$\text{blue}$$。

<img src="{{ site.baseurl }}/assets/images/arc-consistency-ex-3.png" alt="Arc consistency example 3" />

弧一致性通常使用 AC-3 算法（弧一致性算法 #3）实现，其伪代码如下：

<img src="{{ site.baseurl }}/assets/images/arc-consistency-pseudo.png" alt="AC-3 pseudocode" />

AC-3 算法的最坏情况时间复杂度为 $$O(ed^3)$$，其中 $$e$$ 是弧（有向边）的数量，$$d$$ 是最大定义域的大小。总的来说，弧一致性是一种比前向检查更全面的定义域修剪技术，并且会导致更少的回溯，但需要运行更多的计算来强制执行。因此，在决定为你试图解决的 CSP 实现哪种过滤技术时，考虑这种权衡是很重要的。

作为一个关于一致性的有趣的补充说明，弧一致性是更广义的一致性概念 **k-一致性 (k-consistency)** 的子集，当强制执行 k-一致性时，保证对于 CSP 中的任何 $$k$$ 个节点集合，对任何 $$k-1$$ 个节点子集的一致赋值保证第 $$k$$ 个节点将至少有一个一致的值。这个想法可以通过 **强 k-一致性 (strong k-consistency)** 的想法进一步扩展。一个强 $$k$$-一致的图具有这样的属性：任何 $$k$$ 个节点的子集不仅是 $$k$$-一致的，而且也是 $$k-1, k-2, \dots, 1$$-一致的。不足为奇的是，在 CSP 上施加更高程度的一致性计算成本更高。在这个广义的一致性定义下，我们可以看到弧一致性等同于 $$2$$-一致性。
