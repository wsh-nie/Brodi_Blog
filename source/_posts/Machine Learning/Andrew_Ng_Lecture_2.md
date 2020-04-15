---
title: "吴恩达·机器学习课程（二）"
date: 2020-04-11T20:21:17+08:00
draft: false
description: "第二周学习内容：多特征量线性回归，特征缩放，正则方程"
tags: 
- courses notes
- machine learning
categories: 
- machine learning
mathjax: true
---

# 多特征量线性回归

## 基本概念

以研究房屋售价为例，如下表格：  

 Size | Number of bedrooms | Number of floors | Age of home(years) | Price($1000) 
 :--: | :----: | :----: | :----: | :----: 
 2104 | 5 | 1 | 45 | 460 
 1416 | 3 | 2 | 40 | 232  
 1534 | 3 | 2 | 30 | 315  
 852 | 2 | 1 | 36 | 178  
 ... | ... | ... | ... | ...  
 
> $n$ 表示特征量的数量  
> 
> $X^{(i)}$ 表示训练集中的第i个样例  
> 
> $X_j^{(i)}$ 表示训练集中的第i个样例的第j个特征量  

比如上表中，训练集中的输入参数可以用矩阵X表示：

$$
X=\left [\begin{matrix} 
2014 & 5 & 1 & 45 \\\\\\
1416 & 3 & 2 & 40 \\\\\\
1534 & 3 & 2 & 30 \\\\\\
852 & 2 & 1 & 36
\end{matrix}
\right]
$$

第二个样例可以用矩阵的第二行表示为：
$$
X^{(2)}=
\left[\begin{matrix} 
1416 & 3 & 2 & 40 
\end{matrix} \right]
$$

$X_3^{(2)}$ 表示的是第2个样例中的第3个特征量的值，也就是`2`

## 预测函数  

由于特征量增多，所以我们的预测函数的形式也随之改变。  

我们在单一特征量中使用的预测函数是$h_{\theta}(x) = \theta_0 + \theta_1x$  

而在多特征量中，我们的预测函数的格式如下：

$$
h_{\theta}(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + ... + \theta_n x_n
$$

上文中预测房价的预测函数应该是： $h_{\theta}(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 + \theta_4 x_4$

为了表示方便，我们假定$x_0 = 1$，即每个样例的特征向量`x`是一个从`0`开始标记的`n+1`维的向量，即：  
$$ 
x=\left[ \begin{matrix} 
x_0 & x_1 & ... & x_n
\end{matrix}\right] 
$$

另外，我们把参数$\theta$也看作一个`n+1`维的向量：  
$$
\theta = \left[ \begin{matrix}  \theta_0 \\\\\\
\theta_1 \\\\\\
... \\\\\\
\theta_n
\end{matrix}\right]
$$

所以以上形式可以用行列式表示为：$h_\theta(x)=x \times \theta$

$$
h_{\theta}(x) = \left[
\begin{matrix}
x_0 & x_1 & ... & x_n
\end{matrix}
\right] \times \left[
\begin{matrix}
\theta_0 \\\\\\
\theta_1 \\\\\\
... \\\\\\
\theta_n
\end{matrix}
\right]
$$

## 代价函数

对`n`个特征量的代价函数可表示为：

$$
\begin{aligned}
J(\theta_0,\theta_1,...,\theta_n) &= \frac{1}{2m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})^2 \\\\\\ 
&=\frac{1}{2m} \sum_{i=1}^{m} ((\theta_0 * x^{(0)} + \theta_1 * x^{(1)} + ... + \theta_n * x^{(n)}) - y)^2
\end{aligned}
$$

对我们任意一个样例而言，都可以获得一个假设函数的值，我们将整个样本的假设函数计算出来的值用一个`m * 1`的向量表示：

$$h = X \times \theta$$  

我们的参数$y$也是一个`m * 1`的向量，于是我们计算代价函数也可以用线性代数表示为：  
 
$$J=\frac{1}{2m} sum(\ (h-y)  \ \mbox{.^2}  )$$  

其中，$\mbox{.^2}$是对向量中的每个元素进行平方处理，$sum$函数用于对矩阵中所有元素求和。  

## 多特征线性回归的梯度下降算法

上一周的课程可知，多特征线性回归的梯度下降算法中最重要的也就是保持所有的$\theta$是同步更新的。  

即要求同步计算以下式子：

> $\theta_0 := \theta_0 - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)}) x_0^{(i)}$  
>  
> $\theta_1 := \theta_1 - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)}) x_1^{(i)}$  
>  
> $\theta_2 := \theta_2 - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)}) x_2^{(i)}$  
>  
>  ...  

如上，已知$\theta$是一个`n+1`的向量，$\alpha$是一个标量，我们可以将以上的形式用线性代数表示为：

$$
\theta := \theta - \alpha \delta
$$

其中，$\delta$也是一个`n+1`维的向量，抽取其中的第一项进行分析：
$$
\begin{aligned}
\delta_0 &= \frac{1}{m} \sum_{i=0}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})x_0^{(i)} \\\\\\
&= \frac{1}{m} x_0^T \times (h - y)  
\end{aligned} 
$$

即:

$$
\delta = \left[ \begin{matrix}
\frac{1}{m} x_0^T \times (h - y) \\\\\\
\frac{1}{m} x_1^T \times (h - y) \\\\\\
... \\\\\\
\frac{1}{m} x_n^T \times (h - y) 
\end{matrix} \right]
$$

由行列式的计算法则可进一步将上式简化为：

$$
\delta = \frac{1}{m} X^T \times (h - y)
$$

所以，我们可以直接用线性代数表示$\theta$：

$$
\theta := \theta - \alpha \frac{1}{m}  X^T \times (h - y)
$$

# 梯度下降算法的一些实操技巧

## 特征缩放

为什么要进行特征缩放？因为不同特征的取值范围各有不同，将特征的取值范围缩放到大致相同的范围可以使得梯度下降算法更快的收敛。  

课程讲述的是使用均值归一化`Mean Normalization`来进行特征缩放，这样能保证特征均值大约为0，其算法为：

$$
x = \frac{x-\mu}{\sigma}
$$

其中，$\mu$一般取样本中对应的特征的平均值, $\sigma$可以是特征对应的最大值与最小值的差值，也可以是样本中对应特征的标准差。

## 如何选择合适的$\alpha$

当$\alpha$过大时，我们可能无法求出合适的解，当$\alpha$太小时，收敛速度有太慢，但一定时可以求解的。

所以可以先随机选择一个$\alpha$来进行试探，然后再进行调整，那么问题就变成如何确定一个$\alpha$是有效的。

吴恩达老师推荐的方法是对梯度下降算法的每次迭代算出的代价函数的值进行绘图，查看其是否呈下降形式，如是则可尝试增大$\alpha$否则必须减少。

# 一些进阶问题

## 如何定义特征

举个例子，你一直房子的长度和宽度，你该如何选择特征？

第一，你可以将长和宽看多两个特征进行分析问题；第二，你可以把房子的面积当作特征进行分析；第三，你还可以分成靠街的一边和不靠街的一边当作特征来进行分析。这都取决于你怎么定义特征。

## 多项式回归

线性回归是最简单的问题，一般情况下还会出现多项式回归`Polynomial regression`问题。

举个例子，分别以$x$，$x^2$，$x^3$为特征，我们的假设函数是：

$$
h_{\theta}(x) = \theta_0 + \theta_1(x) + \theta_2(x)^2 + \theta_3(x)^3
$$

当$x$的取值范围是$[1,1000]$时，$x^3$的取值范围就达到了$[1,10^9]$，在这里也知道进行特征缩放的必要性了。

## 正则方程

梯度下降算法是从数值方向进行解决问题，还可以从分析上进行解决问题，正则方程`Normal Equation`就是求解$\theta$的一个直接解法。

在前文中我们分析了代价函数的线性代数表示方式，在这里我们可用进一步用行列式计算法则取代我们在前文中使用的`Octave`函数。  
$$\begin{aligned}
J(\theta) &= \frac{1}{2m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)}) - y^{(i)})^2\\\\\\ 
 &= \frac{1}{2m} sum( (X\theta -y) \ \mbox{.^2}) \\\\\\
 &= \frac{1}{2m} (X\theta-y)^T(X\theta-y) \\\\\\
 &= \frac{1}{2m} (\theta^TX^T -y^T)(X\theta-y) \\\\\\
 &= \frac{1}{2m} (\theta^TX^TX\theta - \theta^TX^Ty - y^TX\theta + y^Ty)
\end{aligned} 
$$

代价函数对$\theta$进行求导:
$$\begin{aligned}
\nabla_{\theta} J(\theta) &= \nabla_{\theta} \frac{1}{2m} (\theta^T X^T X \theta - \theta^T X^T y - y^T X \theta + y^T y) \\\\\\
 &= \frac{1}{2m} (X^T X \theta + X^T X \theta - X^T y - X^T y) \\\\\\
 &= \frac{1}{m} (X^TX\theta - X^Ty)
\end{aligned} 
$$
令导数为`0`即可求出最小值的解:
$$
X^TX\theta - X^Ty = 0 
$$
$$
\theta = (X^TX)^{-1}X^Ty
$$

**注意** $X^TX$不可逆可能是两种情况：

1. 出现冗余的特征量，也就是矩阵出现线性相关
2. 特征量过多，也就是$m \leqslant n$

## 梯度下降法与正则方程的比较

梯度下降算法的特点：

- 需要选择合适的学习速率$\alpha$  
- 需要多次迭代  
- 当$n$特别大的时候依然可以很好的运行  


正则方程的特点：
- 不需要选择学习速率$\alpha$
- 不需要多次迭代
- 需要计算$(X^TX)^{-1}$，时间复杂度为$O(n^3)$
- 当n很大的时候，非常慢

---

矩阵求导法则：
$$
\begin{aligned}
\nabla_x x^Tb &= \nabla_x \left[\begin{matrix}
x_1 & x_2 &  \cdots & x_n 
\end{matrix} \right] \times  \left[\begin{matrix}
b_1 \\\\\\ b_2 \\\\\\  \vdots \\\\\\ b_n 
\end{matrix} \right] \\\\\\
 &= \nabla_{x_i} \sum_{i=1}^{n} x_i b_i \\\\\\
 &= b
\end{aligned}
$$

$$
\begin{aligned}
\nabla_x Ax &= \nabla_x \left[\begin{matrix}
a_{11} & a_{12} &  \cdots & a_{1n} \\\\\\
a_{21} & a_{22} &  \cdots & a_{2n} \\\\\\
\vdots & \vdots &  \ddots & \vdots \\\\\\
a_{n1} & a_{n2} &  \cdots & a_{nn} 
\end{matrix} \right] 
\times  \left[\begin{matrix}
x_1 \\\\\\ x_2 \\\\\\  \vdots \\\\\\ x_n 
\end{matrix} \right] \\\\\\
 &= \left[\begin{matrix}
\nabla_{x_i} \sum_{i=1}^{n} a_{1i}x_i \\\\\\
     \nabla_{x_i} \sum_{i=1}^{n} a_{2i}x_i \\\\\\
     \vdots \\\\\\
     \nabla_{x_i} \sum_{i=1}^{n} a_{ni}x_i
\end{matrix} \right] \\\\\\
 &= A^T
\end{aligned}
$$

参考文章：
[线性回归与最小二乘法](https://zhuanlan.zhihu.com/p/36910496)

---