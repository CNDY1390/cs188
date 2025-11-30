---
title: 10.2 逻辑语言
parent: 10. 逻辑
nav_order: 2
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 10.2 逻辑语言 (The Language of Logic)

就像任何其他语言一样，逻辑语句是用特殊的**语法 (syntax)** 编写的。每个逻辑语句都是关于一个可能为真或可能不为真的世界的**命题 (proposition)** 的代码。例如，语句“地板是熔岩”在我们的智能体世界中可能为真，但在我们的世界中可能不为真。我们可以通过用**逻辑连接词 (logical connectives)** 将更简单的语句串联在一起来构建复杂的语句，从而创建像“你可以从 Big C 看到整个校园*并且*徒步旅行是学习之余健康的休息”这样的语句。语言中有五个逻辑连接词：

- $$\neg$$, **非 (not)**: $$\neg P$$ 为真*当且仅当 (iff)* $$P$$ 为假。原子语句 $$P$$ 和 $$\neg P$$ 被称为**文字 (literals)**。
- $$\wedge$$, **与 (and)**: $$A \wedge B$$ 为真*当且仅当* $$A$$ 为真且 $$B$$ 为真。一个“与”语句被称为**合取 (conjunction)**，其组成命题称为**合取项 (conjuncts)**。
- $$\vee$$, **或 (or)**: $$A \vee B$$ 为真*当且仅当* $$ A $$ 为真或 $$ B $$ 为真。一个“或”语句被称为**析取 (disjunction)**，其组成命题称为**析取项 (disjuncts)**。
- $$\Rightarrow$$, **蕴涵 (implication)**: $$A \Rightarrow B$$ 为真，除非 $$A$$ 为真且 $$B$$ 为假。
- $$\Leftrightarrow$$, **双条件 (biconditional)**: $$A \Leftrightarrow B$$ 为真*当且仅当* $$A$$ 和 $$B$$ 都为真或都为假。

<img src="{{ site.baseurl }}/assets/images/truth_table.png" alt="Truth Table" />
