---
title: 9.8 多类逻辑回归
parent: 9. 机器学习
nav_order: 8
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 9.8 多类逻辑回归 (Multi-Class Logistic Regression)

在多类逻辑回归中，我们希望将数据点分类为 $$K$$ 个不同的类别，而不仅仅是两个。因此，我们希望构建一个模型，输出新数据点属于 $$K$$ 个可能类别中每一个的估计概率。出于这个原因，我们使用 **softmax 函数** 代替逻辑函数，它对具有特征 $$\mathbf{x}$$ 的新数据点具有标签 $$i$$ 的概率建模如下：

$$P(y=i|\mathbf{f}(\mathbf{x});\mathbf{w}) = \frac{e^{\mathbf{w}_i^T \mathbf{f}(\mathbf{x})}}{\sum_{k=1}^K e^{\mathbf{w}_k^T \mathbf{f}(\mathbf{x})}}$$

注意这些概率估计加起来为 1，因此它们构成了一个有效的概率分布。我们估计参数 $$\mathbf{w}$$ 以最大化我们观察到数据的可能性（似然）。假设我们观察到了 $$n$$ 个标记的数据点 ($$\mathbf{x}_i, y_i$$)。似然定义为我们样本的联合概率分布，用 $$\ell(\mathbf{w}_1, \ldots, \mathbf{w}_K)$$ 表示，并由下式给出：

$$\ell(\mathbf{w}_1, \ldots, \mathbf{w}_K) = \prod_{i=1}^{n} P(y_i | \mathbf{f}(\mathbf{x}_i); \mathbf{w})$$

为了计算最大化似然的参数 $$\mathbf{w}_i$$ 的值，我们计算似然函数关于每个参数的梯度，将其设为零，并求解未知参数。如果不可能有解析解，我们计算似然的梯度并使用梯度上升来获得最佳值。

简化这些计算的一个常用技巧是首先对似然函数取对数，这将把乘积分解为求和并简化梯度计算。我们可以这样做，因为对数是一个严格递增的函数，并且变换不会影响函数的最大化点。
<p>
</p>
对于似然函数，我们需要一种方法来表达概率 $$P(y_i | \mathbf{f}(\mathbf{x}_i); \mathbf{w})$$，其中 $$y \in \{1, \ldots, K\}$$。所以对于每个数据点 $$i$$，我们定义 $$K$$ 个参数 $$t_{i,k}$$, $$k = 1, \ldots, K$$，使得如果 $$y_i = k$$ 则 $$t_{i,k} = 1$$，否则为 $$0$$。因此，我们现在可以如下表达似然：

<p>
</p>

$$\ell(\mathbf{w}_1, \ldots, \mathbf{w}_K) = \prod_{i=1}^{n} \prod_{k=1}^K \left( \frac{e^{\mathbf{w}_k^T \mathbf{f}(\mathbf{x}_i)}}{\sum_{\ell=1}^K e^{\mathbf{w}_\ell^T \mathbf{f}(\mathbf{x}_i)}} \right)^{t_{i,k}}$$

<p>
</p>
我们也得到对数似然：
<p>
</p>

<p>
</p>

$$\log \ell(\mathbf{w}_1, \ldots, \mathbf{w}_K) = \sum_{i=1}^{n} \sum_{k=1}^K t_{i,k} \log \left( \frac{e^{\mathbf{w}_k^T \mathbf{f}(\mathbf{x}_i)}}{\sum_{\ell=1}^K e^{\mathbf{w}_\ell^T \mathbf{f}(\mathbf{x}_i)}} \right)$$
<p>
</p>

现在我们有了目标的表达式，我们必须估计 $$\mathbf{w}_i$$ 以使它们最大化该目标。

在多类逻辑回归的例子中，关于 $$\mathbf{w}_j$$ 的梯度由下式给出：

<p>
</p>
$$\nabla_{\mathbf{w}_j} \log \ell(\mathbf{w}) = \sum_{i=1}^{n} \nabla_{\mathbf{w}_j} \sum_{k=1}^K t_{i,k} \log \left( \frac{e^{\mathbf{w}_k^T \mathbf{f}(\mathbf{x}_i)}}{\sum_{\ell=1}^K e^{\mathbf{w}_\ell^T \mathbf{f}(\mathbf{x}_i)}} \right) = \sum_{i=1}^{n} \left( t_{i,j} - \frac{e^{\mathbf{w}_j^T \mathbf{f}(\mathbf{x}_i)}}{\sum_{\ell=1}^K e^{\mathbf{w}_\ell^T \mathbf{f}(\mathbf{x}_i)}} \right) \mathbf{f}(\mathbf{x}_i)$$
<p>
</p>

其中我们使用了 $$\sum_k t_{i,k} = 1$$ 的事实。
