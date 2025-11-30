---
title: 10.4 命题逻辑推理
parent: 10. 逻辑
nav_order: 4
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 10.4 命题逻辑推理 (Propositional Logical Inference)

逻辑之所以有用且强大，是因为它赋予了我们从已知事物中得出新结论的能力。为了定义推理问题，我们首先需要定义一些术语。

如果语句 $$A$$ 为真的所有模型中，$$B$$ 也为真，我们说语句 $$A$$ **蕴涵 (entails)** 另一个语句 $$B$$，我们将这种关系表示为 $$A \models B$$。注意，如果 $$A \models B$$，那么 $$A$$ 的模型是 $$B$$ 的模型的子集 ($$M(A)\subseteq M(B)$$)。推理问题可以表述为弄清楚是否 $$KB \models q$$，其中 $$KB$$ 是我们的逻辑语句知识库，$$q$$ 是某个查询。例如，如果 Elicia 发誓再也不踏足 Crossroads，我们可以推断出在寻找朋友一起吃晚饭时我们不会在那里找到她。

我们利用两个有用的定理来证明蕴涵：

1. $$A \models B$$ **当且仅当** $$A \Rightarrow B$$ 是有效的。

    通过证明 $$A \Rightarrow B$$ 是有效的来证明蕴涵被称为**直接证明 (direct proof)**。

2. $$A \models B$$ **当且仅当** $$A \wedge \neg B$$ 是不可满足的。

    通过证明 $$A \wedge \neg B$$ 是不可满足的来证明蕴涵被称为**反证法 (proof by contradiction)**。

## 10.4.1 模型检测 (Model Checking)

检查是否 $$KB \models q$$ 的一个简单算法是枚举所有可能的模型，并检查是否在 $$KB$$ 为真的所有模型中，$$q$$ 也为真。这种方法被称为**模型检测 (model checking)**。在符号数量可行的语句中，可以通过画出**真值表 (truth table)** 来进行枚举。

对于命题逻辑系统，如果有 $$N$$ 个符号，则有 $$2^N$$ 个模型需要检查，因此该算法的时间复杂度为 $$O(2^N)$$，而在一阶逻辑中，模型的数量是无限的。事实上，命题蕴涵问题已知是 co-NP 完全的。虽然最坏情况下的运行时间不可避免地是问题规模的指数函数，但在实践中有些算法可以更快地终止。我们将讨论两种用于命题逻辑的模型检测算法。

第一种由 Davis, Putnam, Logemann 和 Loveland 提出（我们将称之为 **DPLL 算法**），本质上是对可能模型的深度优先回溯搜索，并带有三个减少过度回溯的技巧。该算法旨在解决可满足性问题，即给定一个语句，找到所有符号的有效赋值。正如我们提到的，蕴涵问题可以归约为可满足性问题（证明 $$A \wedge \neg B$$ 不可满足），具体来说，DPLL 接收 CNF 形式的问题。可满足性可以表述为约束满足问题，如下所示：让变量（节点）为符号，约束为 CNF 施加的逻辑约束。然后 DPLL 将继续为符号赋值真值，直到找到满足的模型，或者在不违反逻辑约束的情况下无法分配符号，此时算法将回溯到上一个有效赋值。然而，DPLL 对简单的回溯搜索进行了三项改进：

1. **提前终止 (Early Termination)**：如果任何符号为真，则子句为真。因此，甚至在所有符号被赋值之前，语句就可能被认为是真的。此外，如果任何单个子句为假，则语句为假。在所有变量被赋值之前提前检查整个语句是否可以被判断为真或假，可以防止不必要地在子树中徘徊。
2. **纯符号启发式 (Pure Symbol Heuristic)**：纯符号是在整个语句中仅以其正形式（或仅以其负形式）出现的符号。纯符号可以立即被赋值为真或假。例如，在语句 $$(A \vee B) \wedge (\neg B \vee C) \wedge (\neg C \vee A)$$ 中，我们可以将 $$A$$ 识别为唯一的纯符号，并可以立即将 A 赋值为真，将满足问题简化为仅找到 $$(\neg B \vee C)$$ 的满足赋值。
3. **单位子句启发式 (Unit Clause Heuristic)**：单位子句是只有一个文字的子句，或者是具有一个文字和许多假的析取。在单位子句中，我们可以立即为文字赋值，因为只有一个有效赋值。例如，对于单位子句 $$(B \vee false \vee \cdots \vee false)$$ 为真，$$B$$ 必须为真。

<img src="{{ site.baseurl }}/assets/images/DPLL_alg.png" alt="DPLL Algorithm" />

## 10.4.2 DPLL: 示例

假设我们有以下合取范式 (CNF) 的语句：

$$(\neg N \vee \neg S) \land (M \vee Q \vee N) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P \vee N) \land (\neg R \vee \neg L) \land (S)$$

我们想使用 DPLL 算法来确定它是否可满足。假设我们使用固定的变量顺序（字母顺序）和固定的值顺序（先真后假）。

在对 DPLL 函数的每次递归调用中，我们跟踪三件事：
- **model** 是我们迄今为止分配的符号及其值的列表。
- **symbols** 是仍需分配的未分配符号的列表。
- **clauses** 是 CNF 中的子句（析取）列表，在此调用或将来的 DPLL 递归调用中仍需考虑。

我们通过调用带有空 **model**（尚未分配符号）、包含原始语句中所有符号的 **symbols** 和包含原始语句中所有子句的 **clauses** 的 DPLL 开始。

我们的初始 DPLL 调用如下所示：
- **model**: $$\{\}$$
- **symbols**:  $$[L, M, N, P, Q, R, S]$$
- **clauses**: $$(\neg N \vee \neg S) \land (M \vee Q \vee N) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P \vee N) \land (\neg R \vee \neg L) \land (S)$$

首先，我们应用提前终止：我们检查给定当前模型，是否每个子句都为真，或者至少有一个子句为假。由于模型尚未分配任何符号，我们还不知道哪些子句为真或假。

接下来，我们检查纯文字。没有仅以非否定形式出现的符号，也没有仅以否定形式出现的符号，因此没有我们可以简化的纯文字。例如，$$N$$ 不是纯文字，因为第一个子句使用否定的 $$\neg N$$，而第二个子句使用非否定的 $$N$$。

接下来，我们检查单位子句（只有一个符号的子句）。有一个单位子句 $$S$$。为了使整个语句为真，我们知道 $$S$$ 必须为真（没有其他方法可以满足该子句）。因此，我们可以进行另一次 DPLL 调用，在我们的模型中将 $$S$$ 赋值为真，并从仍需赋值的符号列表中删除 $$S$$。

我们的第二次 DPLL 调用如下所示：

- **model**: $$\{ {\color{red} S: T} \}$$
- **symbols**:  $$[L, M, N, P, Q, R]$$
- **clauses**: $$(\neg N \vee \neg S) \land (M \vee Q \vee N) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P \vee N) \land (\neg R \vee \neg L) \land (S)$$

首先，我们可以通过代入我们模型中的新赋值（$$S$$ 为真，$$\neg S$$ 为假）来简化子句：

$$(\neg N \vee {\color{red} F}) \land (M \vee Q \vee N) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P \vee N) \land (\neg R \vee \neg L) \land ({\color{red} T})$$

$$(\neg N) \land (M \vee Q \vee N) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P \vee N) \land (\neg R \vee \neg L)$$

有了新的简化子句，我们可以检查提前终止。我们仍然没有足够的信息来断定所有语句都为真，或者至少有一个语句为假。

接下来，我们检查纯文字。如前所述，没有仅以非否定形式出现的符号，也没有仅以否定形式出现的符号。

接下来，我们检查单位子句。有一个单位子句 $$(\neg N)$$。为了使整个语句为真，$$(\neg N)$$ 必须为真，所以 $$N$$ 必须为假。

因此，我们可以进行另一次 DPLL 调用，在我们的模型中将 $$N$$ 赋值为假，并从仍需赋值的符号列表中删除 $$N$$。我们还可以使用我们从这次 DPLL 调用中计算出的简化子句（我们在其中简化了 $$S$$）。

我们的第三次 DPLL 调用如下所示：
- **model**: $$\{S:T, {\color{red} N:F}\}$$
- **symbols**:  $$[L, M, P, Q, R]$$
- **clauses**: $$(\neg N) \land (M \vee Q \vee N) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P \vee N) \land (\neg R \vee \neg L)$$

我们在这次调用中做的第一件事是通过代入我们模型中的新赋值（$$N$$ 为假，$$\neg N$$ 为真）来简化子句：

$$({\color{red} T}) \land (M \vee Q \vee {\color{red} F}) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P \vee {\color{red} F}) \land (\neg R \vee \neg L)$$

$$(M \vee Q) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

有了新的简化子句，我们检查提前终止，然后我们检查纯文字。如前所述，我们都没有找到。

接下来，我们检查单位子句。我们没有找到任何只剩下一个符号的子句。

此时，我们需要尝试为一个变量赋值。根据我们固定的变量顺序，我们将首先分配 $$M$$，根据我们固定的值顺序，我们将首先尝试使 $$M$$ 为真。如果分配 $$M$$ 为真导致不可满足的语句，那么我们需要回溯并尝试将 $$M$$ 分配为假。如果分配 $$M$$ 为假也导致不可满足的语句，那么我们将知道整个语句是不可满足的。换句话说，我们现在将进行两次 DPLL 递归调用，一次 $$M$$ 为真，一次 $$M$$ 为假，并检查是否有任何一个产生可满足的赋值。

在 $$M$$ 为真的分支上的第一次 DPLL 调用中，我们将 $$M$$ 为真添加到我们的模型中，并使用上一次调用的简化子句：
- **model**: $$\{S:T, N:F, {\color{red} M:T}\}$$
- **symbols**:  $$[L, P, Q, R]$$
- **clauses**: $$(M \vee Q) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

首先，我们通过代入我们模型中的新赋值（$$M$$ 为真）来简化子句：

$$({\color{red} T} \vee Q) \land (L \vee {\color{red} F}) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

$$(L) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

有了新的简化子句，我们检查提前终止；如前所述，我们没有找到。然而，我们确实找到了一个纯文字，$$\neg Q$$（回想一下，因为没有 $$Q$$ 的实例，只有 $$\neg Q$$ 的实例，这算作纯文字）。我们将 $$Q$$ 设置为假，以便 $$\neg Q$$ 可以为真并继续。

在 $$M$$ 为真的分支上的第二次 DPLL 调用中：
- **model**: $$\{S:T, N:F, M:T, {\color{red} Q:F}\}$$
- **symbols**:  $$[L, P, R]$$
- **clauses**: $$(L) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

我们相应地简化我们的子句：

$$(L) \land (L \vee {\color{red} T}) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

$$(L) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

检查提前终止和纯文字，我们都没有找到。我们确实找到了单位子句 $$(L)$$，我们可以将其设置为真。

在 $$M$$ 为真的同一分支上的下一次调用中，我们现在有：
- **model**: $$\{S:T, N:F, M:T, Q:F, {\color{red} L:T}\}$$
- **symbols**:  $$[P, R]$$
- **clauses**: $$(L) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

让我们简化我们的子句：

$$({\color{red} T}) \land ({\color{red} F} \vee \neg P) \land (R \vee P) \land (\neg R \vee {\color{red} F})$$

$$(\neg P) \land (R \vee P) \land (\neg R)$$

检查提前终止和纯文字，我们什么也没找到。当检查单位子句时，我们找到 $$(\neg P)$$。让我们将整个表达式设置为真，即设置 $$P$$ 为假，以便进行下一次 DPLL 调用。

我们的下一次调用如下进行：
- **model**: $$\{S:T, N:F, M:T, Q:F, L:T, {\color{red} P:F}\}$$
- **symbols**:  $$[R]$$
- **clauses**: $$(\neg P) \land (R \vee P) \land (\neg R)$$

我们用 $$P$$ 设置为假来简化并得到子句：

$$({\color{red} T}) \land (R \vee {\color{red} F}) \land (\neg R)$$
$$(R) \land (\neg R)$$

我们检查提前终止。我们注意到这个语句同时有 $$R$$ 和 $$\neg R$$，它们不能同时满足。此时，我们可以说这个语句是不可满足的。

因为 $$M$$ 为真的分支以不可满足的语句结束，我们回溯到分配 $$M$$ 为真之前的点，并进行一次 $$M$$ 为假的 DPLL 调用。我们在 $$M$$ 为假的分支上的第一次 DPLL 调用：
- **model**: $$\{S:T, N:F, {\color{red} M:F}\}$$
- **symbols**:  $$[L, P, Q, R]$$
- **clauses**: $$(M \vee Q) \land (L \vee \neg M) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

我们通过代入我们模型中的新赋值（$$M$$ 为假）来简化子句：

$$({\color{red} F} \vee Q) \land (L \vee {\color{red} T}) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

$$(Q) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

我们无法提前终止，也没有找到任何纯文字。我们找到一个单位子句 $$Q$$，所以我们进行另一次 DPLL 调用，其中 $$Q$$ 为真（并从我们的符号列表中删除）。

我们在 $$M$$ 为假的分支上的第二次 DPLL 调用：
- **model**: $$\{S:T, N:F, M:F, {\color{red} Q:T}\}$$
- **symbols**:  $$[L, P, R]$$
- **clauses**: $$(Q) \land (L \vee \neg Q) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

将新赋值（$$Q$$ 为真）代入我们的子句：

$$({\color{red} T}) \land (L \vee {\color{red} F}) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

$$(L) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

我们无法提前终止，也没有找到任何纯文字。我们找到一个单位子句 $$L$$，所以我们进行另一次 DPLL 调用，其中 $$L$$ 为真（并从我们的符号列表中删除）。

我们在 $$M$$ 为假的分支上的第三次 DPLL 调用：
- **model**: $$\{S:T, N:F, M:F, Q:T, {\color{red} L:T}\}$$
- **symbols**:  $$[P, R]$$
- **clauses**: $$(L) \land (\neg L \vee \neg P) \land (R \vee P) \land (\neg R \vee \neg L)$$

将新赋值（$$L$$ 为真）代入我们的子句：

$$({\color{red} T}) \land ({\color{red} F} \vee \neg P) \land (R \vee P) \land (\neg R \vee {\color{red} F})$$

$$(\neg P) \land (R \vee P) \land (\neg R)$$

我们无法提前终止，也没有找到任何纯文字。我们找到两个单位子句 $$(\neg P)$$ 和 $$(\neg R)$$。根据我们的变量顺序，我们首先选择 $$P$$，因此我们进行另一次 DPLL 调用，其中 $$P$$ 为假（并从我们的符号列表中删除）。

我们在 $$M$$ 为假的分支上的第三次 DPLL 调用：
- **model**: $$\{S:T, N:F, M:F, Q:T, L:T, {\color{red} P:F}\}$$
- **symbols**:  $$[R]$$
- **clauses**: $$(\neg P) \land (R \vee P) \land (\neg R)$$

将新赋值（$$P$$ 为假）代入我们的子句：

$$({\color{red} T}) \land (R \vee {\color{red} F}) \land (\neg R)$$

$$(R) \land (\neg R)$$

我们检查提前终止。我们注意到这个语句同时有 $$R$$ 和 $$\neg R$$，它们不能同时满足。此时，我们可以说这个语句是不可满足的。

因为 $$M$$ 为真的赋值导致了不可满足的语句，而 $$M$$ 为假的赋值也导致了不可满足的语句，我们可以得出结论，整个语句是不可满足的，我们完成了。
