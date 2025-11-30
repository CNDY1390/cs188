---
title: 10.3 命题逻辑
parent: 10. 逻辑
nav_order: 3
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 10.3 命题逻辑 (Propositional Logic)

像其他语言一样，逻辑也有多种方言。我们将介绍两种：命题逻辑和一阶逻辑。**命题逻辑 (Propositional logic)** 是由**命题符号 (proposition symbols)** 组成的语句，可能由逻辑连接词连接。命题符号通常表示为单个大写字母。每个命题符号代表关于世界的一个原子命题。**模型 (model)** 是对所有命题符号的真或假的赋值，我们可以将其视为“可能的世界”。例如，如果我们有命题 A = “今天下雨了” 和 B = “我忘了带伞”，那么可能的模型（或“世界”）是：

1. {A=true, B=true} (“今天下雨了，我忘了带伞。”)
2. {A=true, B=false} (“今天下雨了，我没忘带伞。”)
3. {A=false, B=true} (“今天没下雨，我忘了带伞。”)
4. {A=false, B=false} (“今天没下雨，我没忘带伞。”)

一般来说，对于 $$N$$ 个符号，有 $$2^N$$ 个可能的模型。如果一个语句在所有这些模型中都为真（例如语句 _True_），我们说它是**有效 (valid)** 的；如果至少有一个模型使其为真，我们说它是**可满足 (satisfiable)** 的；如果在任何模型中都不为真，我们说它是**不可满足 (unsatisfiable)** 的。例如，语句 $$A \wedge B$$ 是可满足的，因为它在模型 1 中为真，但不是有效的，因为它在模型 2、3、4 中为假。另一方面，$$\neg A \wedge A$$ 是不可满足的，因为没有 $$A$$ 的选择返回 True。

下面是一些有用的逻辑等价式，可用于将语句简化为更易于处理和推理的形式。

<img src="{{ site.baseurl }}/assets/images/logical_equivalences.png" alt="Logical Equivalences" />

命题逻辑中一个特别有用的语法是**合取范式 (conjunctive normal form)** 或 **CNF**，它是子句的合取，每个子句是文字的析取。它具有一般形式 $$(P_1 \vee \cdots \vee P_i) \wedge \cdots \wedge (P_j \vee \cdots \vee P_n)$$，即它是“或”的“与”。正如我们将看到的，这种形式的语句适合某些分析。重要的是，每个逻辑语句都有一个逻辑上等价的合取范式。这意味着我们可以通过将这些 CNF 语句“与”在一起，将知识库中包含的所有信息（这只是不同语句的合取）制定为一个大的 CNF 语句。

CNF 表示在命题逻辑中特别重要。在这里，我们将看到一个将语句转换为 CNF 表示的示例。假设我们有语句 $$A\Leftrightarrow (B\vee C)$$ 并且我们想将其转换为 CNF。推导基于图 7.10 中的规则。

1. **消除** $$\Leftrightarrow$$：使用**双条件消除 (biconditional elimination)**，表达式变为 $$(A\Rightarrow (B\vee C))\wedge ((B\vee C)\Rightarrow A)$$。
2. **消除** $$\Rightarrow$$：使用**蕴涵消除 (implication elimination)**，表达式变为 $$(\neg A\vee B\vee C)\wedge (\neg(B\vee C )\vee A)$$。
3. 对于 CNF 表示，“非”($$\neg$$) 必须只出现在文字上。使用**德摩根 (De Morgan's)** 定律，我们得到 $$(\neg A\vee B\vee C)\wedge ((\neg B\wedge \neg C )\vee A)$$。
4. 作为最后一步，我们应用**分配律 (distributivity law)** 并得到 $$(\neg A\vee B\vee C)\wedge (\neg B\vee A )\wedge (\neg C\vee A)$$。

最终表达式是三个 OR 子句的合取，因此它是 CNF 形式。
