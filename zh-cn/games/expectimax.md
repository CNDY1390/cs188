---
title: 3.3 Expectimax
parent: 3. 博弈
nav_order: 3
layout: page
header-includes:
    \pagenumbering{gobble}
lang: zh-cn
---

# 3.3 Expectimax

我们已经看到了 minimax 是如何工作的，以及运行完整的 minimax 如何让我们针对最优对手做出最优反应。然而，minimax 对其可以应对的情况有一些自然的限制。因为 minimax 认为它正在应对一个最优对手，所以在不能保证对手对代理的行动做出最优反应的情况下，它往往过于悲观。这种情况包括具有固有随机性的场景，如纸牌或骰子游戏，或对手随机移动或次优移动的不可预测的对手。我们将在课程的后半部分讨论 **Markov decision processes**（马尔可夫决策过程）时更详细地讨论具有固有随机性的场景。

这种随机性可以通过 minimax 的一种推广来表示，称为 **expectimax**。Expectimax 在博弈树中引入了 *chance nodes*（机会节点），它不像最小化节点那样考虑最坏情况，而是考虑 *average case*（平均情况）。更具体地说，最小化器只是计算其子节点的最小效用，而机会节点计算 **expected utility**（期望效用）或期望值。我们用 expectimax 确定节点值的规则如下：

$$\forall \text{agent-controlled states}, V(s) = \max_{s'\in successors(s)}V(s')$$

$$\forall \text{chance states}, V(s) = \sum_{s'\in successors(s)}p(s'|s)V(s')$$

$$\forall \text{terminal states}, V(s) = \text{known}$$

<p>
</p>
在上述公式中，$$p(s'|s)$$ 指的是给定的非确定性动作导致从状态 $$s$$ 移动到 $$s'$$ 的概率，或者对手选择导致从状态 $$s$$ 移动到 $$s'$$ 的动作的概率，具体取决于游戏的具体情况和所考虑的博弈树。从这个定义中，我们可以看到 minimax 只是 expectimax 的一个特例。最小化节点只是机会节点，它们将其值最低的子节点的概率设为 1，将所有其他子节点的概率设为 0。通常，选择概率是为了正确反映我们要建模的游戏状态，但我们将在以后的笔记中更详细地介绍这个过程是如何工作的。目前，可以公平地假设这些概率只是固有的游戏属性。

Expectimax 的伪代码与 minimax 非常相似，只有一些小的调整来考虑期望效用而不是最小效用，因为我们将最小化节点替换为机会节点：

<img src="{{ site.baseurl }}/assets/images/expectimax-pseudocode.png" alt="Expectimax Pseudocode" />

在我们继续之前，让我们快速浏览一个简单的例子。考虑下面的 expectimax 树，其中机会节点由圆形节点表示，而不是最大化器/最小化器的向上/向下三角形。

<img src="{{ site.baseurl }}/assets/images/unfilled-expectimax.png" alt="Unfilled Expectimax" />

为了简单起见，假设每个机会节点的所有子节点的发生概率均为 $$\frac{1}{3}$$。因此，根据我们的 expectimax 值确定规则，我们看到从左到右的 3 个机会节点的值为 $$\frac{1}{3} \cdot 3 + \frac{1}{3} \cdot 12 + \frac{1}{3} \cdot 9 = \boxed{8}$$，$$\frac{1}{3} \cdot 2 + \frac{1}{3} \cdot 4 + \frac{1}{3} \cdot 6 = \boxed{4}$$，和 $$\frac{1}{3} \cdot 15 + \frac{1}{3} \cdot 6 + \frac{1}{3} \cdot 0 = \boxed{7}$$。最大化器选择这三个值的最大值 $$\boxed{8}$$，产生如下填充的博弈树：

<img src="{{ site.baseurl }}/assets/images/filled-expectimax.png" alt="Filled Expectimax" />

作为关于 expectimax 的最后一点说明，重要的是要意识到，一般来说，有必要查看机会节点的所有子节点——我们不能像 minimax 那样进行剪枝。与在 minimax 中计算最小值或最大值不同，单个值可能会使 expectimax 计算的期望值任意高或低。但是，当我们对可能的节点值有已知的有限界限时，剪枝是可能的。

## 3.3.1 Mixed Layer Types (混合层类型)

虽然 minimax 和 expectimax 分别要求交替的最大化器/最小化器节点和最大化器/机会节点，但许多游戏仍然不遵循这两种算法要求的确切交替模式。即使在 Pacman 中，在 Pacman 移动之后，通常会有多个幽灵轮流移动，而不是单个幽灵。我们可以通过根据需要非常流畅地将层添加到我们的博弈树中来解决这个问题。在有四个幽灵的游戏的 Pacman 示例中，这可以通过在第二个 Pacman/最大化器层之前有一个最大化器层，后跟 4 个连续的幽灵/最小化器层来完成。事实上，这样做本质上会在所有最小化器之间产生合作，因为它们轮流进一步最小化最大化器可获得的效用。甚至可以将机会节点层与最小化器和最大化器结合起来。如果我们有一个有两个幽灵的 Pacman 游戏，其中一个幽灵表现随机，另一个表现最优，我们可以用交替的最大化器-机会-最小化器节点组来模拟这一点。

<img src="{{ site.baseurl }}/assets/images/mixed-layer.png" alt="Mixed Layer" />

显而易见，节点分层有很大的变化空间，允许开发博弈树和对抗搜索算法，这些算法是针对任何零和博弈的修改后的 expectimax/minimax 混合体。
