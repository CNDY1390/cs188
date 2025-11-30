---
title: 10.8 一阶逻辑推理
parent: 10. 逻辑
nav_order: 8
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 10.8 一阶逻辑推理 (First Order Logical Inference)

使用一阶逻辑，我们以完全相同的方式制定推理。我们想弄清楚是否 $$KB \models q$$，即 $$q$$ 是否在 $$KB$$ 为真的所有模型中都为真。找到解决方案的一种方法是**命题化 (propositionalization)**，即将问题转化为命题逻辑，以便可以使用我们已经列出的技术来解决。每个全称（存在）量词语句都可以转换为语句的合取（析取），其中每个可能的对象都有一个子句，可以替换变量。然后，我们可以使用 SAT 求解器，如 DPLL 或 Walk-SAT，来检查 $$(KB \wedge \neg q)$$ 的（不）可满足性。

这种方法的一个问题是，我们可以进行无限数量的替换，因为我们可以将函数应用于符号的次数没有限制。例如，我们可以根据需要多次嵌套函数 `Classmate(... Classmate(Classmate(Austen))...)`，直到我们引用整个学校。幸运的是，Jacques Herbrand (1930) 证明的一个定理告诉我们，如果一个语句被知识库蕴涵，那么就存在一个仅涉及命题化知识库的*有限*子集的证明。因此，我们可以尝试遍历有限子集，特别是通过嵌套函数应用的迭代加深来搜索，即首先搜索具有常量符号的替换，然后是具有 `Classmate(Austen)` 的替换，然后是具有 `Classmate(Classmate(Austen))` 的替换，...

另一种方法是直接使用一阶逻辑进行推理，也称为**提升推理 (lifted inference)**。例如，给定

$$(\forall x ~ HasAbsolutePower(x) \wedge Person(x) \Rightarrow Corrupt(x)) \wedge Person(John) \wedge HasAbsolutePower(John)$$ 

（“绝对的权力导致绝对的腐败”）。我们可以通过将 $$x$$ 替换为 John 来推断 $$Corrupt(John)$$。此规则称为**广义假言推理 (Generalized Modus Ponens)**。一阶逻辑的前向链接算法重复应用广义假言推理和替换来推断 $$q$$ 或表明它无法被推断。
