---
title: 9.7 逻辑回归
parent: 9. 机器学习
nav_order: 7
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 9.7 逻辑回归 (Logistic Regression)

让我们再次思考一下我们之前的线性回归方法。在其中，我们假设输出是一个数值实数。

如果我们想预测一个分类变量呢？逻辑回归允许我们使用逻辑函数将输入特征的线性组合转换为概率：

$$h_{\mathbf{w}}(\mathbf{x}) = \frac{1}{1 + e^{-\mathbf{w}^T \mathbf{x}}}$$

重要的是要注意，虽然逻辑回归被命名为回归，但这是一种误称。逻辑回归用于解决分类问题，而不是回归问题。

逻辑函数 $$g(z) = \frac{1}{1 + e^{-z}}$$ 经常用于模拟二元输出。注意函数的输出总是在 0 和 1 之间，如下图所示：

<center>
    <img src = "../assets/images/Generalized_logistic_function_A0_K1_B1.5_Q0.5_ν0.5_M0.5.png" alt = "drawing" style = "width:500px;"/>
</center>

直观地说，逻辑函数模拟了数据点属于标签为 $$1$$ 的类别的概率。原因是逻辑函数的输出在 0 和 1 之间，我们希望我们的模型捕捉特征具有特定标签的概率。例如，在我们训练了逻辑回归之后，我们获得新数据点的逻辑函数的输出。如果输出值大于 0.5，我们将其分类为标签 $$1$$；否则，我们将其分类为标签 $$0$$。更具体地说，我们如下模拟概率：

$$P(y = +1 \mid \mathbf{f}(\mathbf{x}); \mathbf{w}) = \frac{1}{1 + e^{-\mathbf{w}^T \mathbf{f}(\mathbf{x})}}$$

$$P(y = -1 \mid \mathbf{f}(\mathbf{x}); \mathbf{w}) = 1 - \frac{1}{1 + e^{-\mathbf{w}^T \mathbf{f}(\mathbf{x})}}$$

其中我们使用 $$\mathbf{f}(\mathbf{x})$$ 来表示特征向量 $$\mathbf{x}$$ 的函数（通常是恒等函数），分号 $$`;`$$ 表示概率是参数权重 $$\mathbf{w}$$ 的函数。

需要注意的一个有用属性是逻辑函数的导数是：

$$g'(z) = g(z)(1 - g(z))$$

我们如何训练逻辑回归模型？逻辑回归的损失函数是 $$L2$$ 损失

$$\text{Loss}(\mathbf{w}) = \frac{1}{2} (\mathbf{y} - h_{\mathbf{w}}(\mathbf{x}))^2$$

由于逻辑回归不可能有解析解，我们通过梯度下降估计未知权重 $$\mathbf{w}$$。为此，我们需要使用微分链式法则计算损失函数的梯度。损失函数关于坐标 $$i$$ 的权重的梯度由下式给出：

$$\frac{\partial}{\partial w_i} \frac{1}{2} (y - h_{\mathbf{w}}(\mathbf{x}))^2 = -(y - h_{\mathbf{w}}(\mathbf{x})) h_{\mathbf{w}}(\mathbf{x}) (1 - h_{\mathbf{w}}(\mathbf{x})) x_i$$

其中我们使用了逻辑函数 $$g(z) = \frac{1}{1 + e^{-z}}$$ 的梯度满足 $$g'(z) = g(z)(1 - g(z))$$ 的事实。然后我们可以使用梯度下降估计权重，然后进行预测，如上所述。
