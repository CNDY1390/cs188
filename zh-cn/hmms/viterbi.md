---
title: 8.3 维特比算法
parent: 8. HMMs
nav_order: 3
layout: page
header-includes:
    \pagenumbering{gobble}
---
# 8.3 维特比算法 (Viterbi Algorithm)

在前向算法中，我们使用递归来求解 $$P(X_N|e_{1:N})$$，即给定目前观察到的证据变量，系统可能处于的状态的概率分布。与隐马尔可夫模型相关的另一个重要问题是：*给定目前观察到的证据变量，系统遵循的最可能的隐藏状态序列是什么？* 换句话说，我们想求解 $$\arg\max_{x_{1:N}} P(x_{1:N}|e_{1:N}) = \arg\max_{x_{1:N}} P(x_{1:N},e_{1:N})$$。这个轨迹也可以使用维特比算法通过动态规划来求解。

<p>
</p>
该算法包括两次传递：第一次在时间上向前运行，并计算给定目前观察到的证据，到达每个（状态，时间）元组的最佳路径的概率。第二次在时间上向后运行：首先找到位于概率最高路径上的终止状态，然后沿着通往该状态的路径（这必然是最佳路径）在时间上向后遍历。

为了可视化该算法，考虑下面的**状态格子 (state trellis)**，这是一个随时间变化的状态和转移图：

<img src="{{ site.baseurl }}/assets/images/trellis.png" alt="State Trellis" />
<p>
</p>
在这个有两个可能的隐藏状态（晴天或雨天）的 HMM 中，我们想计算从 $$X_1$$ 到 $$X_N$$ 的最高概率路径（每个时间步的状态分配）。从 $$X_{t-1}$$ 到 $$X_t$$ 的边上的权重等于 $$P(X_t|X_{t-1})P(E_t|X_t)$$，路径的概率是通过取其边权重的*乘积*来计算的。权重公式中的第一项表示特定转移的可能性，第二项表示观察到的证据与结果状态的拟合程度。

回想一下：
$$P(X_{1:N},e_{1:N}) = P(X_1)P(e_1|X_1)\prod_{t=2}^N P(X_t|X_{t-1})P(e_t|X_t)$$
前向算法计算（直到归一化）
$$P(X_N,e_{1:N}) = \sum_{x_1,..,x_{N-1}} P(X_N, x_{1:N-1},e_{1:N})$$
在维特比算法中，我们想计算
$$\arg\max_{x_1,..,x_{N}}P(x_{1:N},e_{1:N})$$

以找到隐藏状态序列的最大似然估计。注意，乘积中的每一项正是层 $$t-1$$ 到层 $$t$$ 之间边权重的表达式。所以，沿着格子上的路径的权重乘积给出了给定证据的路径概率。

我们可以求解所有可能隐藏状态的联合概率表，但这会导致指数级的空间成本。给定这样一个表，我们可以使用动态规划在多项式时间内计算最佳路径。然而，因为我们可以使用动态规划来计算最佳路径，我们不一定需要在任何给定时间拥有整个表。

定义 $$m_t[x_t] = \max_{x_{1:t-1}} P(x_{1:t},e_{1:t})$$，或者从任何 $$x_0$$ 开始并看到目前为止的证据，在时间 $$t$$ 到达给定 $$x_t$$ 的路径的最大概率。这与通过格子从步骤 $$1$$ 到 $$t$$ 的最高权重路径相同。还要注意
\begin{align}
m_t[x_t] &= \max_{x_{1:t-1}} P(e_t|x_t)P(x_t|x_{t-1})P(x_{1:t-1},e_{1:t-1})\\
&= P(e_t|x_t)\max_{x_{t-1}} P(x_t|x_{t-1})\max_{x_{1:t-2}}P(x_{1:t-1},e_{1:t-1})\\
&= P(e_t|x_t)\max_{x_{t-1}} P(x_t|x_{t-1})m_{t-1}[x_{t-1}].
\end{align}
这表明我们可以通过动态规划递归地计算所有 $$t$$ 的 $$m_t$$。这使得确定最可能路径的最后一个状态 $$x_N$$ 成为可能，但我们仍然需要一种方法来回溯以重建整个路径。让我们定义 $$a_t[x_t] = P(e_t|x_t)\arg\max_{x_{t-1}} P(x_t|x_{t-1})m_{t-1}[x_{t-1}] = \arg\max_{x_{t-1}} P(x_t|x_{t-1})m_{t-1}[x_{t-1}]$$ 来跟踪通往 $$x_t$$ 的最佳路径上的最后一次转移。我们现在可以概述该算法。

<pre><code>
Result: Most likely sequence of hidden states x<sub>1:N</sub>
/* Forward pass
for t=1 to N:
    for x<sub>t</sub> in X:
        if t=1:
            m<sub>t</sub>[x<sub>t</sub>] = P(x<sub>t</sub>)P(e<sub>0</sub>|x<sub>t</sub>)
        else:
            a<sub>t</sub>[x<sub>t</sub>] = argmax<sub>x<sub>{t-1}</sub></sub> P(x<sub>t</sub>|x<sub>t-1</sub>)m<sub>t-1</sub>[x<sub>t-1</sub>];
            m<sub>t</sub>[x<sub>t</sub>] = P(e<sub>t</sub>|x<sub>t</sub>)P(x<sub>t</sub>|a<sub>t</sub>[x<sub>t</sub>])m<sub>t-1</sub>[a<sub>t</sub>[x_t]];
        
    

/* Find the most likely path's ending point
x<sub>N</sub> = argmax<sub>xN</sub> m<sub>N</sub>[x<sub>N</sub>];
/* Work backwards through our most likely path and find the hidden states
For t=N to 2:
    x<sub>t-1</sub> = a<sub>t</sub>[x<sub>t</sub>];

</code></pre>

注意，我们的 $$a$$ 数组定义了一组 $$N$$ 个序列，每个序列都是通往特定结束状态 $$x_N$$ 的最可能序列。一旦我们完成前向传递，我们查看 $$N$$ 个序列的可能性，选择最好的一个，并在后向传递中重建它。因此，我们在多项式空间和时间内计算了对我们证据的最可能解释。
