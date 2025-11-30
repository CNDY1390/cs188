---
title: 10.5 定理证明
parent: 10. 逻辑
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 10.5 定理证明 (Theorem Proving)

另一种方法是将推理规则应用于 $$KB$$ 以证明 $$KB \models q$$。例如，如果我们的知识库包含 $$A$$ 和 $$A \Rightarrow B$$，那么我们可以推断出 $$B$$（此规则称为**假言推理 (Modus Ponens)**）。
前面提到的两个算法使用事实 ii.)，通过将 $$A \wedge \neg B$$ 写成 CNF 并证明它是可满足的或不可满足的。

我们也可以使用三个推理规则来证明蕴涵：

1. 如果我们的知识库包含 $$A$$ 和 $$A \Rightarrow B$$，我们可以推断出 $$B$$ (**假言推理 (Modus Ponens)**)。
2. 如果我们的知识库包含 $$A \wedge B$$，我们可以推断出 $$A$$。我们也可以推断出 B。(**与消除 (And-Elimination)**)。
3. 如果我们的知识库包含 $$A$$ 和 $$B$$，我们可以推断出 $$A \wedge B$$ (**归结 (Resolution)**)。

最后一条规则构成了**归结算法 (resolution algorithm)** 的基础，该算法迭代地将其应用于知识库和新推断出的语句，直到推断出 $$q$$（在这种情况下我们已经证明了 $$KB \models q$$），或者没有剩下的东西可以推断（在这种情况下 $$KB \not\models q$$）。

然而，在我们的知识库只有文字（符号本身）和蕴涵的特殊情况下：$$(P_1 \wedge \cdots \wedge P_n \Rightarrow Q) \equiv (\neg P_1 \vee \cdots \vee \neg P_2 \vee Q)$$，我们可以在与知识库大小成线性的时间内证明蕴涵。一种算法，$$\textbf{前向链接 (forward chaining)}$$，迭代遍历每个蕴涵语句，其中 $$\textbf{前提 (premise)}$$（左侧）已知为真，将 $$\textbf{结论 (conclusion)}$$（右侧）添加到已知事实列表中。这重复直到 $$q$$ 被添加到已知事实列表中，或者无法推断出更多内容。

<img src="{{ site.baseurl }}/assets/images/Forward-Chaining-algorithm.png" alt="Forward Chaining Algorithm" />
