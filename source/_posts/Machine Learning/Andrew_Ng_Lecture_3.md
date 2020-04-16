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

![图1](/images/Lecture3_1.png)

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

![图2](/images/Lecture3_2.png)

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
 &= -\frac{1}{m} [ \sum_{i=1}^{m} y^{(i)}log(h_{\theta}(x^{(i)})) + (1-y^{(i)})log(1-h_{\theta}(x^{(i)}))] \\\\\\
 &= -\frac{1}{m} [\sum_{i=1}^{m} y^{(i)} log(\frac{h_{\theta}(x^{(i)})}{1-h_{\theta}(x^{(i)})}) + log(1-h_{\theta}(x^{(i)})) ] \\\\\\
 &= -\frac{1}{m} [\sum_{i=1}^{m} - y^{(i)} x^{(i)} \theta  + log(1-h_{\theta}(x^{(i)}))]
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
\frac {\partial}{\partial \theta_j} J(\theta) = \frac{1}{m} (h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)}
$$ 

所以，对应的梯度下降算法为：

$$\begin{matrix}
Repeat \lbrace  & \ \\\\\\
\ & \theta_j := \theta_j - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)} \\\\\\ 
\rbrace & \ 
\end{matrix}
$$

好像跟线性回归没啥区别呀，但是需要注意的是，两个假设函数是不同的。

## 高级优化

## 多元分类：一对多

# 正则化

## 过度拟合问题

## 代价函数

## 正则化线性回归

## 正则化逻辑回归
