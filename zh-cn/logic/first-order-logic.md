---
title: 10.7 一阶逻辑
parent: 10. 逻辑
nav_order: 7
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 10.7 一阶逻辑 (First Order Logic)

逻辑的第二种方言，**一阶逻辑 (first-order logic, FOL)**，比命题逻辑更具表现力，并使用对象作为其基本组件。使用一阶逻辑，我们可以描述对象之间的关系并将函数应用于它们。每个对象由**常量符号 (constant symbol)** 表示，每个关系由**谓词符号 (predicate symbol)** 表示，每个函数由**函数符号 (function symbol)** 表示。

下表总结了一阶逻辑语法。

<img src="{{ site.baseurl }}/assets/images/ch8_pic-1.png" alt="First Order Logic Syntax" />

一阶逻辑中的**项 (Terms)** 是指代对象的逻辑表达式。项的最简单形式是常量符号。然而，我们不想为每个可能的对象定义不同的常量符号。例如，如果我们想指代 John 的左腿和 Richard 的左腿，我们可以使用像 `Leftleg(John)` 和 `Leftleg(Richard)` 这样的**函数符号**来做到这一点。函数符号只是命名对象的另一种方式，并不是实际的函数。

一阶逻辑中的**原子语句 (Atomic sentences)** 是对对象之间关系的描述，如果关系成立，则为真。原子语句的一个例子是 `Brother(John, Richard)`，它由一个谓词符号后跟括号内的一列项组成。一阶逻辑的**复合语句 (Complex sentences)** 类似于命题逻辑中的复合语句，是由逻辑连接词连接的原子语句。

自然地，我们希望有方法来描述对象的整个集合。为此，我们使用**量词 (quantifiers)**。**全称量词 (universal quantifier)** $$\forall$$，意思是“对于所有”，**存在量词 (existential quantifier)** $$\exists$$，意思是“存在”。

例如，如果我们将世界上的对象集合设为所有辩论的集合，语句 $$\forall a, ~ TwoSides(a)$$ 可以翻译为“每场辩论都有两面”。如果我们将世界上的对象集合设为人，语句 $$\forall x, \exists y, ~ SoulMate(x, y)$$ 将意味着“对于所有人，外面都有一个人是他们的灵魂伴侣。” **匿名变量 (anonymous variables)** $$a, x, y$$ 是对象的替身，可以被**替换 (substituted)** 为实际对象，例如，将 Laura 替换为我们第二个例子中的 $$x$$ 将导致“外面有一个人是 Laura 的灵魂伴侣”的陈述。

全称量词和存在量词分别是表达所有对象上的合取或析取的便捷方式。由此可见，它们也遵守德摩根定律（注意合取和析取之间的类似关系）：

<img src="{{ site.baseurl }}/assets/images/quantifiers.png" alt="Quantifiers" />

最后，我们使用**等号 (equality symbol)** 来表示两个符号指代同一个对象。例如，令人难以置信的语句 $$\left(Wife(Einstein) = First Cousin(Einstein) \wedge Wife(Einstein) = Second Cousin(Einstein)\right)$$ 是真的！

与命题逻辑不同，在命题逻辑中，模型是对所有命题符号的真或假的赋值，一阶逻辑中的模型是将所有常量符号映射到对象，将谓词符号映射到对象之间的关系，将函数符号映射到对象的函数。如果在映射下语句描述的关系为真，则语句在模型下为真。虽然命题逻辑系统的模型数量总是有限的，但如果对象的数量不受限制，一阶逻辑系统的模型数量可能是无限的。

这两种逻辑方言允许我们以不同的方式描述和思考世界。使用命题逻辑，我们将世界建模为一组真或假的符号。在这个假设下，我们可以将可能的世界表示为一个向量，每个符号为 1 或 0。这种世界的二进制视图被称为**因子化表示 (factored representation)**。使用一阶逻辑，我们的世界由相互关联的对象组成。这种世界的第二种面向对象的视图被称为**结构化表示 (structured representation)**，在许多方面更具表现力，并且更紧密地符合我们自然用来谈论世界的语言。
