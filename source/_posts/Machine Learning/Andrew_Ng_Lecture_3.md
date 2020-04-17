---
layout: _post/Machine Learning
title: "吴恩达·机器学习课程（三）"
date: 2020-04-14 22:59:46
draft: false
description: "第三周学习内容：分类与逻辑回归，正则化"
tags: 
- courses notes
- machine learning
categories: 
- machine learning
mathjax: true
---

# 逻辑回归

## 分类

机器学习里面的**监督学习**和**非监督学习**的区别是：
> 假设函数是否能对输入的例子产生一个确切的输出值

监督学习里面的**回归**和**分类**问题的区别是：
> 回归问题的输出是一个准确数值，分类问题的输出是离散值

肿瘤是恶性的还是良性的？对我们的训练集样例而言，$y \in \lbrace 0,1 \rbrace$，其中`0`表示肿瘤属于良性，`1`表示肿瘤属于恶性。这就是一个简单的一元分类问题。当然，也可以用`1`、`2`、`3`、`4`，分别表示阴、晴、雨、雪，这是一个多元分类问题。

![图1](/images/Machine_Learning_Lecture3_1.png)

如上图，我们采用线性回归方法对训练集进行学习，可以认为当$h_{\theta}(x) \geq 0.5$就认为预测值为`1`，当$h_{\theta}(x) < 0.5$就认为预则值为`0`。

一元分类问题中，$y \in \lbrace 0,1 \rbrace$，但是由线性回归得出的假设函数的取值会超过这个范围，在分类问题中，使用的是**逻辑回归**(`Logistic Regression`)来生成假设函数。

## 假设陈述

逻辑回归模型中，想要假设函数$0 \leq h_{\theta}(x) \leq 1$，就需要对假设函数进行如下改造：
$$
\left . \begin{array}{1} h_{\theta}(x)=g(x\theta) \\\\\\ g(z)=\frac{1}{1+e^{-z}} \end{array} \right \rbrace \Rightarrow h_{\theta}(x)=\frac{1}{1+e^{-x\theta}}
$$

而且，$h_{\theta}(x)$表示的是评估对于输入$x$输出为`1`的可能性。

用数学语言表示就是：

$$
h_{\theta}(x)= P(y=1 | x; \theta)  对于参数\theta，给定x，y=1的可能性
$$

由数学性质可知：

$$
P(y=1 | x;\theta) + P(y=0 | x;\theta) = 1
$$

$$
P(y=0 | x;\theta) = 1 - P(y=1 | x;\theta)
$$

## 决策界限

对于上面给出的假设函数，可以绘制函数图形如下图：

![图2](/images/Machine_Learning_Lecture3_2.png)

可知，当$z \geq 0$，也就是$x\theta \geq 0$，此时$g(z) \geq 0.5$也就是$h_\theta(x) \geq 0.5$，这是可以认为预测值为：$y=1$。同样，当$z < 0$，也就是$x\theta < 0$，此时$g(z) < 0.5$也就是$h_\theta(x) < 0.5$，这是可以认为预测值为：$y=0$。

什么是决策界限呢？就是用于区分给定输入$x$，输出是`0`还是`1`的边界情况。如上面，也就是$x\theta = 0$就是决策界限。

考虑一个非线性的决策界限，现有一个假设函数为$h_{\theta}(x) = g(\theta_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_1^2 + \theta_4 x_2^2)$，当$\theta = \left[ \begin{matrix} -1 & 0 & 0 & 1 & 1 \end{matrix}\right]^T$，当$x\theta = 0$，决策界限为：$x_1^2 + x_2^2 = 1$。

当然，更高次的假设函数就会带来更复杂的决策边界。

## 代价函数

已经确定了假设函数的形式，那么如何确定逻辑回归的参数$\theta$呢？

训练集形式为：${(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),...,(x^{(m)},y^{(m)})}$

同样令$x_0=1$，用矩阵形式表示训练集：

$$
\left[ 
\begin{matrix}
x_0^{(1)} & x^{(1)} \\\\\\
x_0^{(2)} & x^{(2)} \\\\\\
\vdots & \vdots \\\\\\
x_0^{(m)} & x^{(m)} 
\end{matrix}
\right]
$$

线性回归中代价函数形式如下：

$$J(\theta)= \frac{1}{m} \sum_{i=1}^{m} \frac{1}{2} (h_{\theta}(x^{(i)}) - y^{(i)})^2$$

在这里换一种形式，首先记$Cost(h_{\theta}(x^{(i)},y^{(i)})) = \frac{1}{2} (h_{\theta}(x^{(i)})-y^{(i)})^2$，则代价函数可以表示为：

$$
J(\theta) = \frac{1}{m} \sum_{i=1}^{m} Cost(h_{\theta}(x^{(i)}) , y^{(i)})
$$

在前面课程中已经知道，只有当代价函数使凸函数的时候，梯度下降算法才能更好的求出全局最优解。

所以根据**凸优化**(`Convex Optimization`)，可进一步处理：

$$
Cost(h_{\theta}(x),y) = \begin{cases} - log(h_{\theta}(x)) & \text{if  y=1} \\\\\\ - log(1-h_{\theta}(x)) & \text{if  y=0} \end{cases}
$$

根据此函数性质可知，如果$y=1$，当$h_{\theta}(x) =1$时，$Cost \to 0$，反过来，当$h_{\theta}(x) =0$时，$Cost \to \infty$。也就是说，如果$h_{\theta}(x)=0$但实际上$y=1$，这种设定将给学习算法施加一个代价非常大的惩罚。

## 简化代价函数与梯度下降算法

将上面的分段函数可以组合成一个表达式，如下：

$$
Cost(h_{\theta}(x),y) = -ylog(h_{\theta}(x)) -(1-y)log(1-h_{\theta}(x))
$$

那么，代价函数可表示为：

$$
\begin{aligned}
J(\theta) &= \frac{1}{m} \sum_{i=1}^{m} Cost(h_{\theta}(x^{(i)}),y^{(i)}) \\\\\\
 &= -\frac{1}{m} \bigg [ \sum_{i=1}^{m} y^{(i)}log(h_{\theta}(x^{(i)})) + (1-y^{(i)})log(1-h_{\theta}(x^{(i)})) \bigg ] \\\\\\
 &= -\frac{1}{m} \bigg [\sum_{i=1}^{m} y^{(i)} log(\frac{h_{\theta}(x^{(i)})}{1-h_{\theta}(x^{(i)})}) + log(1-h_{\theta}(x^{(i)})) \bigg ] \\\\\\
 &= -\frac{1}{m} \bigg [\sum_{i=1}^{m} - y^{(i)} x^{(i)} \theta  + log(1-h_{\theta}(x^{(i)})) \bigg ]
\end{aligned}
$$

通过最小化代价函数，就可以确定参数$\theta$。

计算代价函数的梯度$\frac {\partial}{\partial \theta_j} J(\theta)$

$$
\frac {\partial}{\partial \theta_j} (- y^{(i)} x^{(i)} \theta) = -y^{(i)}x_j^{(i)}
$$

$$
\begin{aligned} 
\frac {\partial}{\partial \theta_j} log(1-h_{\theta}(x^{(i)})) &= \frac {\partial}{\partial \theta_j} log( \frac{e^{x^{(i)}\theta}}{1+e^{x^{(i)}\theta}} ) \\\\\\
&= \frac{1+e^{x^{(i)}\theta}}{e^{x^{(i)}\theta}} \frac{\partial}{\partial \theta_j}(\frac{e^{x^{(i)}\theta}}{1+e^{x^{(i)}\theta}}) \\\\\\
&= \frac{1+e^{x^{(i)}\theta}}{e^{x^{(i)}\theta}} \frac{e^{x^{(i)}\theta} x_j^{(i)}}{(1+e^{x^{(i)}\theta})^2} \\\\\\
&= \frac{1}{1+e^{x^{(i)}\theta}} x_j^{(i)} \\\\\\
&= h_{\theta}(x^{(i)}) x_j^{(i)}
\end{aligned}
$$

即：

$$
\frac {\partial}{\partial \theta_j} J(\theta) = \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)}
$$ 

记$h = sigmoid(X\theta) = \frac{1}{1+e^{X\theta}}$，同样可用线性代数简化表示偏导数：

$$
\frac {\partial}{\partial \theta} J(\theta) = \frac{1}{m} X^T  (h-y) 
$$

所以，对应的梯度下降算法为：

$$\begin{aligned}
& Repeat \lbrace  \\\\\\
& \theta_j := \theta_j - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)} \\\\\\ 
& \rbrace  
\end{aligned}
$$

好像跟线性回归没啥区别呀，但是需要注意的是，两个假设函数是不同的。

## 高级优化

对于逻辑回归问题，定义了代价函数，目标就是是的代价函数最小化。在梯度下降算法中，对于给出的$\theta$，通过代码进行计算$J(\theta)$和$\frac{\partial}{\partial \theta_j}J(\theta)$，然后不断地重复。

在线性回归问题中，提出了特征缩放对梯度下降算法进行优化，实际上还有很多比梯度下降算法更优秀的算法，比如**共轭梯度算法**(`Conjugate gradient`)、**BFGS**、**L-BFGS**等。

这些算法比梯度下降算法更好，也更加适合解决大型机器学习算法问题，这些算法可能需要花费大量的时间才能搞懂细节，但庆幸的是，它们大都已经集成在一些库中。

这类优化算法的优缺点也是非常明确的：
>优点：
> - 不需要手动选择$\alpha$
> - 普遍比梯度下降算法更快
> 
>缺点：
> - 更复杂的算法，更难理解细节，也更难进行debug

## 多元分类：一对多

在实际中，多元分类问题更多，那么怎么处理多元分类问题呢？

可以把多元分类问题转化为多个一元分类问题，对于认可一个分类，都可以进行伪分类处理，即分成属于这类和不属于这类，然后对每个分类`i`都进行逻辑回归训练，然后选择$max_{i} \ \ h_{\theta}^{(i)}(x)$当作预测结果。

# 正则化

## 过度拟合问题

![图三](/images/Machine_Learning_Lecture3_3.png)

对于线性回归问题，第一个图表现的是欠拟合(`underfitting`)状态,具有高偏差(`high bias`)；第二图表示的是恰好拟合状态；第三个图表示的是过度拟合(`overfitting`)状态。

![图四](/images/Machine_Learning_Lecture3_4.png)

同样，对于逻辑回归问题，第一个图表现的是欠拟合状态；第二图表示的是恰好拟合状态；第三个图表示的是过度拟合状态，具有高方差(`high variance`)。

当使用大量的特征量，学习获得的假设函数能够很好地拟合训练集，但是这种千方百计地拟合训练集也导致它无法泛化到新的样本中。

简而言之，当特征量过多，而没有足够地数据去控制它来产生一个很好的假设函数，就会出现过度拟合问题。

那么如何解决过度拟合问题呢？有以下操作可以考虑：

>1. 减少特征量地数量
>  - 手动选择保留哪些特征量
>  - 通过模型选择算法来自动选择保留或丢弃某些特征
>2. 正则化
>  - 保留所有的特征量，减少量级或参数$\theta_j$的大小

当拥有大量特征量的时候，正则化是非常有用的，它使得每个特征量在预测$y$上都能够提供微小的贡献

## 代价函数

如上面的线性回归过度拟合问题中，可以在代价函数中对高阶的$\theta_3$和$\theta_4$附加一定的惩罚，比如：

$$
J(\theta) = \frac{1}{2m} \bigg [ \sum_{i=1}^{m} (h_{\theta}(x^{(x)}) - y^{(i)})^2 \bigg ] +1000 \theta_{3}^{2} + 1000 \theta_{4}^{2}
$$

求解$\theta$的时候就需要使得代价函数最小，也就意味着$\theta_3$和$\theta_4$的值要尽量的小。也就意味着假设函数中的高阶量对整个函数的影响越小。

这种思想就是，参数值$\theta$较小就意味着假设函数更简单也就意味着更不容易出现过度拟合。

在上面一个例子中，通过增加惩罚项来使得参数值变小，假设函数可以近似看成二次函数。即缩小参数值可以简化假设函数。

所以，为了假设函数更好的拟合训练集也为了将参数控制的更小以生成简单的假设函数来避免过度拟合，在代价函数中引入惩罚项。

$$
J(\theta) = \frac{1}{2m} \bigg [ \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})^2 + \lambda \sum_{j=1}^{n} \theta_j^2 \bigg ]
$$

$\lambda$是正则化参数，用来控制两个目标的平衡。此外需要注意的是，没有对$\theta_0$施加惩罚项。

那么该如何选择合适的$\lambda$呢？显然，当$\lambda$过大，导致$h_{\theta}(x) \approx \theta_0$，结果就是欠拟合了。


## 正则化线性回归

在线性回归问题中，了解了两种求解策略，那么正则化在这两个算法中又该如何使用呢？

在梯度下降算法中，算法步骤修改为如下：

$$
\begin{aligned}
& Repeat \lbrace \\\\\\ 
& \theta_0  :=  \theta_0 - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})x_{0}^{(i)} \\\\\\
& \theta_{j}  :=  \theta_{j} - \alpha \bigg [ \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})x_{j}^{(i)} -\frac{\lambda}{m} \theta_j \bigg ] \\\\\\
& \rbrace 
\end{aligned}
$$

进一步将$\theta_j$化简：

$$
\theta_{j} := \theta_{j}(1-\alpha \frac{\lambda}{m}) - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}
$$

由上面式子可知，$1-\alpha \frac{\lambda}{m} <1$，对$\theta_j$进行了缩小，后部分与不进行正则化一样。

在正则方程中也产生了改变：

$$
\theta = (X^TX + \lambda 
\begin{bmatrix} 
0 & 0 & 0 & \cdots & 0 \\\\\\  
0 & 1 & 0 & \cdots & 0 \\\\\\
0 & 0 & 1 & \cdots & 0 \\\\\\
\vdots & \vdots & \vdots & \ddots & \vdots \\\\\\
0 & 0 & 0 & \cdots & 1 
\end{bmatrix})^{-1} X^Ty
$$

上式中，即使$XTX$是不可逆的，其构造后的$X^TX + \lambda \begin{bmatrix} 0 & 0 & 0 & \cdots & 0 \\\\\\ 0 & 1 & 0 & \cdots & 0 \\\\\\ 0 & 0 & 1 & \cdots & 0 \\\\\\ \vdots & \vdots & \vdots & \ddots & \vdots \\\\\\ 0 & 0 & 0 & \cdots & 1 \end{bmatrix}$可逆。

## 正则化逻辑回归

在逻辑回归中，同样的在代价函数中加入惩罚项：

$$
J(\theta) = -\frac{1}{m} \bigg [ \sum_{i=1}^{m} y^{(i)}log(h_{\theta}(x^{(i)})) + (1-y^{(i)})log(1-h_{\theta}(x^{(i)}))  \bigg ] + \frac{\lambda}{2m} \sum_{j=1}^{n} \theta_j^2
$$

同样的，在梯度下降和一些高级优化中也会有相应的一些优化。