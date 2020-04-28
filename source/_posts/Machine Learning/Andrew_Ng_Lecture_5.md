---
title: "吴恩达·机器学习课程（五）"
date: 2020-04-20 01:13:37
draft: false
description: "第五周学习内容：神经网络的学习"
tags: 
- courses notes
- machine learning
categories: 
- machine learning
mathjax: true
---

# 神经网络的学习

## 代价函数

将训练集表示为$\lbrace (x^{(1)},y^{(1)}),(x^{(2)}s,y^{(2)}),\cdots,(x^{(m)},y^{(m)}) \rbrace$，$L$表示神经网络层数，$s_l$表示第$l$层神经单元的数量(偏置单元未记录在内)，$s_L$代表最后一层中处理单元的个数。

将神经网络定义为两类：二类分类和多类分类。

二类分类：$s_L=1,y=0 \text{ or } 1$; s

$K$类分类：$s_L=K,y_i=1$表示分到第$i$类，其中$K>2$。


在逻辑回归中，代价函数定义为：

$$
J(\theta) = -\frac{1}{m} \bigg [ \sum_{i=1}^{m} y^{(i)}log(h_{\theta}(x^{(i)})) + (1-y^{(i)})log(1-h_{\theta}(x^{(i)}))  \bigg ] + \frac{\lambda}{2m} \sum_{j=1}^{n} \theta_j^2
$$

在逻辑回归中，只有一个输出变量，又称之为标量(`Scalar`)，也只有一个因变量，但是在神经网络中(特别是多类分类问题)，可以又很多的输出变量，$h_{\theta}(x)$是一个维度为$k$的向量，与训练集中的因变量是同样的维度，所以在神经网络中，对应的代价函数会更复杂。

假设函数定义为$h_{\Theta}(x)$，其中$h_{\Theta}(x) \in \Re^k$，$h_{\Theta}(x)_i$表示第i个输出。

$$
J(\Theta) = -\frac{1}{m} [ \sum_{i=1}^{m} \sum_{k=1}^{K} y_{k}^{(i)} \cdot log(h_\Theta(x^{(i)}))_k +(1-y^{(i)}_k) \cdot log(1-(h_\Theta(x^{(i)}))_k)]
$$

$$
+\frac{\lambda}{2m} \cdot \sum_{l=1}^{L-1} \sum_{i=1}^{s_l} \sum_{j=1}^{s_{l+1}} (\Theta_{ji}^{(l)})^2
$$

代价函数一下变得复杂起来了，但是基本思想还是没变，代价函数用来衡量预测结果与案例实际结果的误差，此处与前面不同点在于，对于每个案例都会给出$K$个预测，然后选择其中可能性最高的一个，将其与实际的$y$进行比较。

正则化处理中，对每层的$\theta_0$都进行了排除。根据$a^{(l+1)}=\Theta^{(l)} \cdot a^{(l)}$可知，$\Theta$的行数由$l+1$层激活单元数量决定，列数由$l$层激活单元数量决定。即：$h_\Theta(x)$与真实值之间的距离为每个样本-每个类输出的加和，对参数进行`regularization`的`bias`项处理所有参数的平方和。

## 反向传播算法

有了代价函数，接下来应该是尽可能地最小化代价函数，也就是需要对代价函数进行计算和对偏导数进行计算。

鉴于其代价函数地复杂性，直接对$\Theta$进行求解偏导是十分困难的，考虑分解求导来求解问题：

$$
\frac{\partial}{\partial \Theta_{ij}^{(l)}}J(\Theta) =\frac{\partial J(\Theta)}{\partial z_j^{(l+1)}} \cdot \frac{\partial z_j^{(l+1)}}{\partial \Theta_{ij}^{(l)}}
$$

先考虑求解$\frac{\partial J(\Theta)}{\partial z_j^{(l)}}$。

记$\delta_{j}^{(l)}=\frac{\partial J(\Theta)}{\partial z_{j}^{(l)}}$，其中$l$表示神经网络中的层数，$j$表示第$l$层的第$j$个神经元，$z_{j}^{(l)}=\sum_{i=1}^{s_l} \Theta_{ji}^{(l)} \cdot a_{i}^{(l)} + a_0^{(l)}$。

考虑最后一层：

$$
\begin{aligned}
\delta_j^{(L)} &=\frac{\partial J(\Theta)}{\partial z_j^{(l)}} \\\\\\
&=\sum_{k=1}^{K} \frac{\partial J}{\partial a_k^{(L)}} \frac{\partial a_k^{(L)}}{\partial z_j^{(L)}} \text{只有k==j时，右边才不为0} \\\\\\
&= \frac{\partial J}{\partial a_j^{(L)}} \frac{\partial a_j^{(L)}}{\partial z_j^{(L)}} \\\\\\
&=  \frac{\partial J}{\partial a_j^{(L)}} g'(z_j^{(L)})
\end{aligned}
$$

$$\begin{aligned}
\frac{\partial J}{\partial a_j^{(L)}} &= (-1) \times [ y_j \cdot \frac{1}{a_j^{(L)}} + (1-y_j) \cdot \frac{-1}{1-a_j^{(L)}} ] = \frac{a_j^{(L)} - y_j }{a_j^{(L)} \cdot (1-a_j^{(L)}) }\\\\\\
g'(z_j^{(L)}) &=g(z_j^{L})\cdot(1-g(z_j^{(L)})) = a_j^{(L)} \cdot (1-a_j^{(L)}) \\\\\\
\therefore \delta_j^{(L)} &= a_j^{(L)} - y_j
\end{aligned}
$$

用向量表示即：$\delta^{(L)} = a^{(L)} -y$。

对于其他层，则有：

$$
\begin{aligned}
\delta_j^{(l)} &= \frac{\partial J}{\partial z_j^{(l)}} \\\\\\
&= \sum_{k=1}^{s_{l+1}} \frac{\partial J}{\partial z_k^{(l+1)}} \cdot \frac{\partial z_k^{(l+1)}}{\partial z_j^{(l)}} \\\\\\
&= \sum_{k=1}^{s_{l+1}} \delta_k^{(l+1)} \cdot \frac{\partial z_k^{(l+1)}}{\partial z_j^{(l)}}
\end{aligned}
$$

其中：

$$
z_k^{(l+1)} = \sum_{p=1}^{s_l}\Theta_{kp}^{(l)}g(z_p^{(l)})+a_0^{(l)}
$$

求偏导：

$$
\frac{\partial z_k^{(l+1)}}{\partial z_j^{(l)}} = \Theta_{kj}^{(l)}g'(z_j^{(l)}) = \Theta_{kj}^{(l)} \cdot [ a_j^{(l)} \cdot (1-a_j^{(l)}) ]
$$

所以：

$$
\delta_j^{(l)} = (\Theta_j^{(l)})^T\delta_j^{(l+1)} \cdot [ a_j^{(l)} \cdot (1-a_j^{(l)}) ]
$$

用向量形式表示即$\delta^{(l)} = (\Theta^{(l)})^T \delta^{(l+1)} \cdot [ a^{(l)} (1-a^{(l)})]$

接下来求解梯度：

$$
\frac{\partial z_j^{l+1}}{\Theta_{ij}^{(l)}} = g(z_j^{(l)}) \text{当且仅当k=i，p=j有值}
$$

所以：

$$
\frac{\partial J(\Theta)}{\partial \Theta_{ij}^{(l)}} = g(z_j^{(l)})\delta_i^{(l+1)} = a_j^{(l)} \cdot \delta_i^{(l+1)}
$$

以上，就是反向传播算法的核心要点，现在可以直接使用反向传播算法来求解梯度了。

先来了解前(正)向传播方法，之前在计算神经网络预测结果时，从第一层开始一层一层进行计算，直到最后一层地$h_\Theta(x)$，这就是前向传播方法，如下图。

![图1](/images/Machine_Learning_Lecture5_1.png)

在正向传播计算的基础上，按照反向传播算法，从最后一层的误差开始计算，然后进一步来计算前一层的误差：

$$
\begin{aligned}
& \delta^{(4)} = a^{(4)}-y \\\\\\
& \delta^{(3)} = (\Theta^{(3)})^T\delta^{(4)} \cdot g'(z^{(3)}) \\\\\\
& \delta^{(2)} = (\Theta^{(2)})^T\delta^{(3)} \cdot g'(z^{(2)})
\end{aligned}
$$

将$\delta_j^{(l)}$记为第$l$层的第$j$个激活单元的预测值与实际值之间的误差。注意，不存在$\delta^{(1)}$，因为第一层是输入变量，不存在误差。

当考虑正则化处理时，同样需要计算每一层的误差单元，而且由于训练集是一个特征矩阵，故需要为整个训练集计算误差单元，此时的误差单元也是一个矩阵，用$\Delta_{ij}^{(l)}$来表示第$l$层的第$i$个激活单元收到第$j$个案例影响而导致的误差。

所以，算法可描述为：
$$\begin{aligned}
\text{for } & i=1:m \lbrace \\\\\\
  &  set a^{(1)} = x^{(i)} \\\\\\
  &  \text{perform foward propagation to computer } a^{(l)} \text{ for } l=2,3,\cdots,L \\\\\\
  &  \text{Using }  \delta^{(L)} = a^{(L)} -y^{(i)} \\\\\\
  &  \text{perform back propagation to computer all previous layer error vector} \\\\\\ 
  &  \Delta_{ij}^{(l)} :=\Delta_{ij}^{(l)} + a_j^{(l)}\delta_{i}^{(l+1)} \\\\\\
\rbrace &  
\end{aligned}
$$

## 反向传播算法的直观理解


## 实现注意：展开参数


## 梯度检验


## 随机初始化


## 综合


## 自动驾驶