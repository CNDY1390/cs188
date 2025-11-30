---
title: 9.5 线性回归
parent: 9. 机器学习
nav_order: 5
layout: page
header-includes:
    \pagenumbering{gobble}
---

# 9.5 线性回归 (Linear Regression)

现在我们将从之前对朴素贝叶斯的讨论转向**线性回归 (Linear Regression)**。这种方法也称为**最小二乘法 (least squares)**，可以追溯到卡尔·弗里德里希·高斯，是机器学习和计量经济学中研究最多的工具之一。

**回归问题 (Regression problems)** 是一种机器学习问题，其中输出是一个连续变量（用 $$y$$ 表示）。特征可以是连续的或分类的。我们将用 $$\mathbf{x} \in \mathbb{R}^n$$ 表示一组特征，其中有 $$n$$ 个特征，即 $$\mathbf{x} = (x^1, \ldots, x^n)$$。

我们使用以下线性模型来预测输出：

$$h_{\mathbf{w}}(\mathbf{x}) = w_0 + w_1 x^1 + \cdots + w_n x^n$$

其中线性模型的权重 $$w_i$$ 是我们想要估计的。权重 $$w_0$$ 对应于模型的截距。有时在文献中，我们在特征向量 $$\mathbf{x}$$ 上添加一个 1，这样我们就可以将线性模型写为 $$\mathbf{w}^T \mathbf{x}$$，其中现在 $$\mathbf{x} \in \mathbb{R}^{n+1}$$。为了训练模型，我们需要一个衡量模型预测输出好坏的指标。为此，我们将使用 $$L_2$$ 损失函数，它使用 $$L_2$$ 范数惩罚预测输出与实际输出的差异。如果我们的训练数据集有 $$N$$ 个数据点，那么损失函数定义如下：

$$Loss(h_{\mathbf{w}}) = \frac{1}{2} \sum_{j=1}^N L_2(y^j, h_{\mathbf{w}}(\mathbf{x}^j)) = \frac{1}{2} \sum_{j=1}^N (y^j - h_{\mathbf{w}}(\mathbf{x}^j))^2 = \frac{1}{2} \left\|\mathbf{y} - \mathbf{X} \mathbf{w}\right\|_2^2$$

注意 $$\mathbf{x}^j$$ 对应于第 $$j$$ 个数据点 $$\mathbf{x}^j \in \mathbb{R}^n$$。添加 $$\frac{1}{2}$$ 只是为了简化解析解的表达式。最后一个表达式是损失函数的等价公式，它使最小二乘推导更容易。量 $$\mathbf{y}$$、$$\mathbf{X}$$ 和 $$\mathbf{w}$$ 定义如下：

$$\mathbf{y} = \begin{bmatrix}
y^1 \\
y^2 \\
\vdots \\
y^N
\end{bmatrix}, \quad
\mathbf{X} = \begin{bmatrix}
1 & x_1^1 & \cdots & x_1^n \\
1 & x^1_2 & \cdots & x^n_2 \\
\vdots & \vdots & \ddots & \vdots \\
1 & x^1_N & \cdots & x^n_N
\end{bmatrix}, \quad
\mathbf{w} = \begin{bmatrix}
w_0 \\
w_1 \\
\vdots \\
w_n
\end{bmatrix}$$

其中 $$\mathbf{y}$$ 是堆叠输出的向量，$$\mathbf{X}$$ 是特征向量的矩阵，其中 $$x_j^i$$ 表示第 $$j$$ 个数据点的第 $$i$$ 个分量。用 $$\hat{\mathbf{w}}$$ 表示的最小二乘解现在可以使用基本的线性代数规则推导出来。更具体地说，我们将通过对损失函数求导并将导数设为零来找到最小化损失函数的 $$\hat{\mathbf{w}}$$。

$$\nabla_{\mathbf{w}} \frac{1}{2} \left\|\mathbf{y} - \mathbf{X} \mathbf{w}\right\|_2^2 = \nabla_{\mathbf{w}} \frac{1}{2} \left(\mathbf{y} - \mathbf{X} \mathbf{w}\right)^T \left(\mathbf{y} - \mathbf{X} \mathbf{w}\right)$$

$$= \nabla_{\mathbf{w}} \frac{1}{2} \left(\mathbf{y}^T \mathbf{y} - \mathbf{y}^T \mathbf{X} \mathbf{w} - \mathbf{w}^T \mathbf{X}^T \mathbf{y} + \mathbf{w}^T \mathbf{X}^T \mathbf{X} \mathbf{w}\right)$$

$$= \nabla_{\mathbf{w}} \frac{1}{2} \left(\mathbf{y}^T \mathbf{y} - 2 \mathbf{w}^T \mathbf{X}^T \mathbf{y} + \mathbf{w}^T \mathbf{X}^T \mathbf{X} \mathbf{w}\right) = -\mathbf{X}^T \mathbf{y} + \mathbf{X}^T \mathbf{X} \mathbf{w}$$

将梯度设为零，我们得到：

$$-\mathbf{X}^T \mathbf{y} + \mathbf{X}^T \mathbf{X} \mathbf{w} = 0 \Rightarrow \hat{\mathbf{w}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$$

获得估计的权重向量后，我们现在可以对新的未见测试数据点进行预测，如下所示：

$$h_{\hat{\mathbf{w}}}(\mathbf{x}) = \hat{\mathbf{w}}^T \mathbf{x}$$
