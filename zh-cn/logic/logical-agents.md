---
title: 10.9 逻辑智能体
parent: 10. 逻辑
nav_order: 9
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 10.9 逻辑智能体 (Logical Agents)

既然我们了解了如何制定我们所知道的以及如何用它进行推理，我们将讨论如何将演绎的力量融入我们的智能体中。智能体应该具备的一个明显能力是根据观察历史和它对世界的了解来弄清楚它处于什么状态（**状态估计 (state-estimation)**）。例如，如果我们告诉智能体熔岩池附近的空气开始闪烁，并且它观察到它面前的空气正在闪烁，它可以推断出危险就在附近。

为了将其过去的观察纳入对其当前位置的估计，智能体需要有时间的概念和状态之间的转换。我们将随时间变化的状态属性称为**流变 (fluents)**，并用时间索引编写流变，例如 $$Hot^t$$ = 时间 $$t$$ 时空气是热的。如果某事导致时间 $$t$$ 时空气变热，或者前一时刻空气是热的并且没有发生改变它的动作，那么时间 $$t$$ 时空气应该是热的。为了表示这一事实，我们可以使用下面**后继状态公理 (successor-state axiom)** 的一般形式：

$$F^{t+1} \Leftrightarrow ActionCausesF^t \vee (F^t \wedge \neg ActionCausesNotF^t)$$

在我们的世界中，转换可以制定为

$$Hot^{t+1} \Leftrightarrow StepCloseToLava^t \vee (Hot^t \wedge \neg StepAwayFromLava^t)$$

用逻辑写出世界的规则后，我们现在可以通过检查某些逻辑命题的可满足性来进行规划！为此，我们构建一个包含有关初始状态、转换（后继状态公理）和目标的信息的语句。（例如 $$InOasis^T \wedge Alive^T$$ 编码了生存并在时间 T 之前到达绿洲的目标）。如果世界的规则已经正确制定，那么找到所有变量的满足赋值将允许我们提取一系列将智能体带到目标的动作。
