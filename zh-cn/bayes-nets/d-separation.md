---
title: "6.5 D-分离"
parent: 6. 贝叶斯网络
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 6.5 D-分离 (D-Separation)

关于一组随机变量，一个有用的问题是一个变量是否独立于另一个变量，或者一个随机变量在给定第三个随机变量的情况下是否条件独立于另一个随机变量。贝叶斯网络对联合概率分布的表示为我们提供了一种通过检查图的拓扑结构来快速回答此类问题的方法。

我们已经提到，**在给定所有父节点的情况下，一个节点条件独立于图中的所有祖先节点。**

我们将介绍连接的三节点双边贝叶斯网络（或三元组）的所有三种典型情况，以及它们表达的条件独立关系。

## 6.5.1 因果链 (Causal Chains)

<img src="{{ site.baseurl }}/assets/images/chain_free.PNG" alt="Causal Chain with no observations" />
*图 1: 无观测的因果链。*

<img src="{{ site.baseurl }}/assets/images/chain_observed.PNG" alt="Causal Chain with Y observed" />
*图 2: 观测到 Y 的因果链。*

图 1 是三个节点的配置，称为**因果链 (causal chain)**。它表达了 $$X$$、$$Y$$ 和 $$Z$$ 上联合分布的以下表示：

$$P(x, y, z) = P(z|y)P(y|x)P(x)$$

重要的是要注意，$$X$$ 和 $$Z$$ 不保证是独立的，如下面的反例所示：

$$P(y|x) = 
\begin{cases} 
      1 & \text{if } x = y \\
      0 & \text{else }
  \end{cases}$$

$$P(z|y) = 
\begin{cases} 
      1 & \text{if } z = y \\
      0 & \text{else }
  \end{cases}$$

<p>
</p>
在这种情况下，如果 $$x = z$$，$$P(z|x) = 1$$，否则为 $$0$$，所以 $$X$$ 和 $$Z$$ 不是独立的。

<p>
</p>
然而，我们可以做出声明 $$X \perp\!\!\!\perp Z | Y$$，如图 2 所示。回想一下，这个条件独立性意味着：
<p>
</p>

$$P(X | Z, Y) = P(X | Y)$$

我们可以如下证明这个陈述：

$$P(X | Z, y) = \frac{P(X, Z, y)}{P(Z, y)}
= \frac{P(Z|y) P(y|X) P(X)}{\sum_{x} P(x, y, Z)}
= \frac{P(Z|y) P(y|X) P(X)}{P(Z|y) \sum_{x} P(y|x)P(x)}
= \frac{P(y|X) P(X)}{\sum_{x} P(y|x)P(x)}
= P(X|y)$$

<p>
</p>
类似的证明可以用于显示 $$X$$ 有多个父节点的情况。总结一下，在因果链配置中，$$X \perp\!\!\!\perp Z | Y$$。

## 6.5.2 共同原因 (Common Cause)

<img src="{{ site.baseurl }}/assets/images/cause_free.PNG" alt="Common Cause with no observations" />

*图 3: 无观测的共同原因。*

<img src="{{ site.baseurl }}/assets/images/cause_observed.PNG" alt="Common Cause with Y observed" />

*图 4: 观测到 Y 的共同原因。*

三元组的另一种可能配置是**共同原因 (common cause)**。它表达了以下表示：

$$P(x, y, z) = P(x|y)P(z|y)P(y)$$

就像因果链一样，我们可以用以下反例分布证明 $$X$$ 不保证独立于 $$Z$$：

$$P(x|y) = 
\begin{cases} 
      1 & \text{if } x = y \\
      0 & \text{else }
  \end{cases}$$

$$P(z|y) = 
\begin{cases} 
      1 & \text{if } z = y \\
      0 & \text{else }
  \end{cases}$$

<p>
</p>
那么如果 $$x = z$$，$$P(x|z) = 1$$，否则为 $$0$$，所以 $$X$$ 和 $$Z$$ 不是独立的。
<p>
</p>
但是 $$X \perp\!\!\!\perp Z | Y$$ 是真的。也就是说，如果观测到 $$Y$$，如图 4 所示，则 $$X$$ 和 $$Z$$ 是独立的。我们可以如下证明这一点：

$$P(X | Z, y) = \frac{P(X, Z, y)}{P(Z, y)} = \frac{P(X|y) P(Z|y) P(y)}{P(Z|y) P(y)} = P(X|y)$$

## 6.5.3 共同结果 (Common Effect)

<img src="{{ site.baseurl }}/assets/images/effect_free.PNG" alt="Common Effect with no observations" />

*图 5: 无观测的共同结果。*

<img src="{{ site.baseurl }}/assets/images/effect_observed.PNG" alt="Common Effect with Y observed" />

*图 6: 观测到 Y 的共同结果。*

它表达了表示：

$$P(x, y, z) = P(y|x,z)P(x)P(z)$$

在图 5 所示的配置中，$$X$$ 和 $$Z$$ 是独立的：$$X \perp\!\!\!\perp Z$$。然而，当以 $$Y$$ 为条件时（图 6），它们不一定独立。举个例子，假设这三个都是二元变量。$$X$$ 和 $$Z$$ 为真和假的概率相等：

$$P(X=true) = P(X=false) = 0.5$$

$$P(Z=true) = P(Z=false) = 0.5$$

并且 $$Y$$ 由 $$X$$ 和 $$Z$$ 是否具有相同的值决定：

$$P(Y | X, Z) = 
\begin{cases} 
  1 & \text{if } X = Z \text{ and } Y = true \\
  1 & \text{if } X \ne Z \text{ and } Y = false \\
  0 & \text{else}
\end{cases}$$

那么如果 $$Y$$ 未被观测到，$$X$$ 和 $$Z$$ 是独立的。但是如果观测到 $$Y$$，那么知道 $$X$$ 将告诉我们 $$Z$$ 的值，反之亦然。所以给定 $$Y$$，$$X$$ 和 $$Z$$ *不是*条件独立的。
<p>
</p>
共同结果可以被视为与因果链和共同原因“相反”——如果未以 $$Y$$ 为条件，$$X$$ 和 $$Z$$ 保证是独立的。但是当以 $$Y$$ 为条件时，$$X$$ 和 $$Z$$ 可能是相关的，这取决于 $$P(Y | X, Z)$$ 的具体概率值。

同样的逻辑适用于以图中 $$Y$$ 的后代为条件的情况。如果观测到 $$Y$$ 的一个后代节点，如图 7 所示，$$X$$ 和 $$Z$$ 不保证是独立的。

<img src="{{ site.baseurl }}/assets/images/effect_children.PNG" alt="Common Effect with child observations" />

*图 7: 观测到子节点的共同结果。*

## 6.5.4 一般情况和 D-分离 (General Case, and D-separation)

我们可以使用前三种情况作为构建块，帮助我们回答具有超过三个节点和两条边的任意贝叶斯网络上的条件独立性问题。我们将问题表述如下：

<p>
</p>
**给定一个贝叶斯网络 $$G$$，两个节点 $$X$$ 和 $$Y$$，以及一组（可能为空的）代表观测变量的节点 $$\{Z_1, \dots Z_k\}$$，以下陈述是否一定为真：$$X \perp\!\!\!\perp Y | \{Z_1, \dots Z_k\}$$？**

<p>
</p>
**D-分离 (D-separation)**（有向分离）是贝叶斯网络图结构的一个属性，它暗示了这种条件独立关系，并概括了我们上面看到的情况。如果一组变量 $$Z_1, \cdots Z_k$$ d-分离 $$X$$ 和 $$Y$$，那么在贝叶斯网络可以编码的所有可能分布中，$$X \perp\!\!\!\perp Y | \{Z_1, \cdots Z_k\}$$。

我们从一个基于从节点 $$X$$ 到节点 $$Y$$ 的可达性概念的算法开始。（**注意：这个算法不完全正确！我们稍后会看到如何修复它。**）

1. 在图中对所有观测节点 $$\{Z_1, \dots Z_k\}$$ 进行阴影处理。
2. 如果存在一条从 $$X$$ 到 $$Y$$ 的无向路径未被阴影节点阻断，则 $$X$$ 和 $$Y$$ 是“连接的”。
3. 如果 $$X$$ 和 $$Y$$ 是连接的，则它们在给定 $$\{Z_1, \dots Z_k\}$$ 的情况下不是条件独立的。否则，它们是。

然而，该算法仅在贝叶斯网络图中没有共同结果结构时才有效，因为如果存在，那么当共同结果中的 $$Y$$ 节点被激活（观测到）时，两个节点是“可达的”。为了对此进行调整，我们得出以下 **d-分离算法**：

1. 在图中对所有观测节点 $$\{Z_1, \dots Z_k\}$$ 进行阴影处理。
2. 枚举从 $$X$$ 到 $$Y$$ 的所有无向路径。
3. 对于每条路径：
    1. 将路径分解为三元组（3 个节点的片段）。
    2. 如果所有三元组都是活跃的，则此路径是活跃的并 *d-连接* $$X$$ 到 $$Y$$。
4. <p></p>如果没有路径 d-连接 $$X$$ 和 $$Y$$，则 $$X \perp\!\!\!\perp Y | \{Z_1, \dots Z_k\}$$。

图中从 $$X$$ 到 $$Y$$ 的任何路径都可以分解为一组 3 个连续节点和 2 条边——每个都称为一个三元组。三元组是活跃的还是不活跃的取决于中间节点是否被观测到。如果路径中的所有三元组都是活跃的，则该路径是活跃的并 *d-连接* $$X$$ 到 $$Y$$，这意味着给定观测节点，$$X$$ 不保证条件独立于 $$Y$$。如果从 $$X$$ 到 $$Y$$ 的所有路径都是不活跃的，则给定观测节点，$$X$$ 和 $$Y$$ 是条件独立的。

---

我们可以使用我们在下面的图中展示的三个典型图来枚举活跃和不活跃三元组的所有可能性。

**活跃三元组**: 

<img src="{{ site.baseurl }}/assets/images/active.PNG" alt="Active triples" />

**不活跃三元组**:

<img src="{{ site.baseurl }}/assets/images/inactive.PNG" alt="Inactive triples" />

## 6.5.5 示例 (Examples)

以下是应用 $$d$$-分离算法的一些示例：

<img src="{{ site.baseurl }}/assets/images/rbtt.png" alt="Example 1" />

此图包含共同结果和因果链典型图。

$$R \perp\!\!\!\perp B$$ -- 保证
<p>
</p>
$$R \perp\!\!\!\perp B | T$$ -- 不保证
<p>
</p>
$$R \perp\!\!\!\perp B | T'$$ -- 不保证
<p>
</p>
$$R \perp\!\!\!\perp T' | T$$ -- 保证

<img src="{{ site.baseurl }}/assets/images/lrbdtt.png" alt="Example 2" />

此图包含所有三个典型图的组合（你能列出它们吗？）。

$$L \perp\!\!\!\perp T' | T$$ -- 保证
<p>
</p>
$$L \perp\!\!\!\perp B$$ -- 保证
<p>
</p>
$$L \perp\!\!\!\perp B | T$$ -- 不保证
<p>
</p>
$$L \perp\!\!\!\perp B | T'$$ -- 不保证
<p>
</p>
$$L \perp\!\!\!\perp B | T, R$$ -- 保证

<img src="{{ site.baseurl }}/assets/images/rtds.png" alt="Example 3" />

此图包含所有三个典型图的组合。
<p>
</p>
$$T \perp\!\!\!\perp D$$ -- 不保证
<p>
</p>
$$T \perp\!\!\!\perp D | R$$ -- 保证
<p>
</p>
$$T \perp\!\!\!\perp D | R, S$$ -- 不保证
