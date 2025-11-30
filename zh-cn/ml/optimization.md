---
title: 9.6 优化
parent: 9. 机器学习
nav_order: 6
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 9.6 优化 (Optimization)

线性回归方法允许我们通过对损失函数求导并将梯度设为零来推导出最优权重的解析解。然而，一般来说，对于给定的目标函数，可能不存在解析解。在这种情况下，我们使用**基于梯度的方法 (gradient-based methods)** 来找到最优权重。其思想是梯度指向目标函数增长最快的方向。我们通过向最陡**上升 (ascent)** 方向移动来最大化函数，通过向最陡**下降 (descent)** 方向移动来最小化函数。

如果目标是我们试图最大化的函数，则使用**梯度上升 (Gradient ascent)**。

**算法 1：** 梯度上升
1. 随机初始化 $$\mathbf{w}$$。
2. 当 $$\mathbf{w}$$ 未收敛时：

$$\mathbf{w} \leftarrow \mathbf{w} + \alpha \nabla_{\mathbf{w}} f(\mathbf{w})$$

如果目标是我们试图最小化的损失函数，则使用**梯度下降 (Gradient descent)**。注意，这与梯度上升的唯一区别在于我们遵循梯度的相反方向。

**算法 2：** 梯度下降
1. 随机初始化 $$\mathbf{w}$$。
2. 当 $$\mathbf{w}$$ 未收敛时：

$$\mathbf{w} \leftarrow \mathbf{w} - \alpha \nabla_{\mathbf{w}} f(\mathbf{w})$$

在开始时，我们随机初始化权重。我们用 $$\alpha$$ 表示学习率，它捕捉了我们向梯度方向移动的步长大小。对于机器学习世界中的大多数函数，很难想出一个最优的学习率值。实际上，我们希望学习率足够大，以便我们快速向正确的方向移动，但同时又要足够小，以便该方法不会发散。机器学习文献中的一种典型方法是以相对较大的学习率开始梯度下降，并随着迭代次数的增加而减小学习率（学习率衰减）。

如果我们的数据集有大量的 $$n$$ 个数据点，那么在梯度下降算法的每次迭代中如上计算梯度可能会过于计算密集。因此，已经提出了像随机梯度下降和小批量梯度下降这样的方法。在**随机梯度下降 (stochastic gradient descent)** 中，在算法的每次迭代中，我们只使用一个数据点来计算梯度。这一个数据点每次都是从数据集中随机采样的。鉴于我们只使用一个数据点来估计梯度，随机梯度下降可能会导致嘈杂的梯度，从而使收敛变得稍微困难一些。**小批量梯度下降 (Mini-batch gradient descent)** 是随机梯度下降和普通梯度下降算法之间的折衷方案，因为它每次使用大小为 $$m$$ 的一批数据点来计算梯度。批量大小 $$m$$ 是用户指定的参数。

让我们看一个在我们之前见过的模型——线性回归上进行梯度下降的例子。回想一下，在线性回归中，我们将损失函数定义为

$$\text{Loss}(h_{\mathbf{w}}) = \frac{1}{2}\left\|\mathbf{y} - \mathbf{X}\mathbf{w}\right\|_2^2$$

线性回归有一个著名的解析解 $$\hat{\mathbf{w}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$$，我们在上一篇笔记中看到了这一点。然而，我们也可以选择通过运行梯度下降来求解最优权重。我们将损失函数的梯度计算为

$$\nabla_{\mathbf{w}} \text{Loss}(h_{\mathbf{w}}) = -\mathbf{X}^T \mathbf{y} + \mathbf{X}^T \mathbf{X} \mathbf{w}$$

然后，我们使用这个梯度写出线性回归的梯度下降算法：

**算法 3**：最小二乘梯度下降
1. 随机初始化 $$\mathbf{w}$$
2. 当 $$\mathbf{w}$$ 未收敛时：

$$\mathbf{w} \leftarrow \mathbf{w} - \alpha (-\mathbf{X}^T \mathbf{y} + \mathbf{X}^T \mathbf{X} \mathbf{w})$$

创建一个线性回归问题并确认解析解与你从梯度下降中获得的收敛解相同是一个很好的练习。
