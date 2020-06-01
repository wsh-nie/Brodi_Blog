---
title: "线性规划"
date: 2020-05-29 18:54:30
draft: false
description: "线性规划；运输问题-表上作业法；指派问题——匈牙利算法；拉格朗日算法与对偶问题；"
tags: 
- Convex Optimization
- Algorithm
categories: 
- Algorithm
mathjax: true
---

# 线性规划

## 基本概念

线性规划(`Linear Programming - LP`)问题是在一组线性约束条件的限制下，求已先行目标函数最大获最小的问题。

其一般形式为：

$$
\begin{align} 
& \underset{x} {min } \text{ }  c^Tx  \tag {1} \\\\\\
& \text{s.t.}  \begin{cases}
Ax \leq b \\\\\\
Aeq \cdot x = beq \\\\\\
lb \leq x \leq ub
\end{cases} \tag {2}
\end{align}
$$

其中，$x$为$n$维列向量的优化参数，$c$为$n$维列向量，也是$x$的系数，$Ax \leq b$表示不等式约束，$Aeq \cdot x = beq$表示等式约束，$lb,ub$表示参数定义域。

满足约束条件(2)的解称为线性规划问题的**可行解**，所有可行解够成的集合称之为问题的**可行域**，记作$R$；而使得目标函数(1)达到最大值的可行解叫**最优解**。

对于一般$n$维空间，有如下定义：

满足线性等式$\sum_{i=1}^{n}a_i x_i=b$的点集称为一个**超平面**；而满足线性不等式$\sum_{i=1}^{n}a_i x_i \leq b$(或$\sum_{i=1}^{n}a_i x_i \geq b$)的点集称为一个**半空间**；若干半空间的交集称之为**多胞形**；有界的多胞形称之为**多面体**。

对于线性规划问题，一般有如下断言：

1. 可行域R可能是空集也可能是非空集合，R可能是有界区域，也可能是无界区域，即线性规划的可行域必为多胞形($\emptyset$也被视为多胞形)；
2. 当R非空时，线性规划可能存在有限最优解，也可以不存在有限最优解；
3. 若线性规划存在有限最优解，则必可找到具有最有目标函数值的可行域R的"顶点"。

二维空间中的顶点可以看成为边界直线的交点，但这一几何概念的推广在一般$n$维空间中的几何意义并不直观，我们采用另一种途径来定义。

$\text{Definition} 1$：称$n$维空间中的区域$R$为一凸集，若$\forall x^1,x^2 \in R$及$\forall \lambda \in (0,1)$，有$\lambda x^1 + (1-\lambda) x^2 \in R$。

事实上，定义1说明凸集中任何两点的连线必在凸集中，如下图。

![图1](/images/ConvexOptimization_LinearProgramming_1.png)


$\text{Definition} 2$：设$R$为$n$维空间中的一个凸集，$R$中的点$x$被称为$R$的一个极点，则不存在$x^1,x^2 \in R$及$\lambda \in (0,1)$，使得$x=\lambda x^1 + (1-\lambda) x^2$。

定义2说明，若x是凸集R的一个极点，则x不能位于R中任意两点的连线上。

## 单纯形法



# 运输问题

## 问题描述

 某商品有$m$个产地、$n$个销地，各产地的产量分别为$a_1,a_2,\cdots,a_m$，各销地的需求量分别为$b_1,b_2,\cdots,b_n$。若该商品由$i$产地运到$j$销地的单位运价为$C_{ij}$ ，问应该如何调
运才能使总运费最省？

该类型的问题的数学模型为：

$$
\begin{align} 
& {min } \text{ }  \sum_{i=1}^{m} \sum_{j=1}^{n} C_{ij}x_{ij}  \tag {3} \\\\\\
& \text{s.t.}  \begin{cases}
\text{ } \sum_{j=1}^{n} x_{ij} = a_{i}, \text{ } i=1,\cdots,m \\\\\\
\sum_{i=1}^{m} x_{ij} = b_{j}, \text{ } j=1,\cdots,m \\\\\\
x_{ij} \leq 0
\end{cases} \tag {4}
\end{align}
$$

这类问题显然也是线性规划问题，同样可以采用单纯形法来求解。但需要注意的是这类问题的约束条件的系数矩阵相当特殊。

$$
\sum_{j=1}^{n} b_j = \sum_{i=1}^{m} \bigg ( \sum_{j=1}^{n} x_{ij} \bigg ) = \sum_{j=1}^{n} \bigg ( \sum_{i=1}^{m} x_{ij} \bigg ) = \sum_{i=1}^{m} a_j
$$

故这类问题可用比较简单的计算方法，即表上作业法。

## 表上作业法



# 指派问题

## 问题描述

拟分配$n$人去干$n$项工作，每人干且仅干一项工作，若分配第$i$人去干第$j$项工作，需花费$C_{ij}$单位时间，问应如何分配工作才能使工人花费的总时间最少？

引入变量$x_{ij}$，若分配i干j工作，则去$x_{ij}=1$，否则去$x_{ij}=0$。则上述指派问题数学模型为：

$$
\begin{align} 
& {min } \text{ }  \sum_{i=1}^{n} \sum_{j=1}^{n} C_{ij}x_{ij}  \tag {5} \\\\\\
& \text{s.t.}  \begin{cases}
\text{ } \sum_{i=1}^{n} x_{ij} = 1 \\\\\\
\sum_{j=1}^{n} x_{ij} = 1 \\\\\\
x_{ij} =0 \text{ or } 1
\end{cases} \tag {6}
\end{align}
$$

上述指派问题的可行解可以用一个矩阵表示，其每行每列均有且只有一个元素为$1$，其余元素均为$0$；可以用$1,\cdots,n$中的一个置换表示。

问题中的变量只能取$0$或$1$，从而是一个`0-1`规划问题。一般的`0-1`规划问题求解极为困难。但指派问题并不难解，其约束方程组的系数矩阵十分特殊(被称为全单位模矩阵，其各阶非零子式均为$±1$)，其非负可行解的分量只能取$0$或$1$，故约束$x_{ij}= 0 \text{ or } 1$可改写为$x_{ij} \leq 0$而不改变其解。此时，指派问题被转化为一个特殊的运输问题，其中$m = n, a_i=b_i=1$。

## 匈牙利算法

# 拉格朗日算法

拉格朗日乘子法(`Lagrange multipliers`)是一种寻找多元函数在一组约束下的极致的方法，也是用去求解线性规划的常用算法。

## 等式约束

先考虑等式约束问题，假定$x$为$d$维向量，在约束条件$g(x) = 0$下欲寻找$x$的某个选取值$x^{\*}$，使目标函数$f(x)$最小。

数学模型可简化如下：

$$
\begin{align} 
& \underset{x} {min } \text{  }  f(x)  \tag {7} \\\\\\
& \text{s.t.  }  g(x) = 0 \tag {8}
\end{align}
$$

从几个角度看，该问题的目标是在由方程$g(x)=0$确定的$d-1$维曲面(由方程组$AX=0$的解可知，$g(x)=0$表示的是$d-1$维曲面)上寻找能使目标函数$f(x)$最小化的解。此时不难得到如下结论：

1. 对于约束曲面上的任意点$x$，该点的梯度$\nabla g(x)$正交于约束曲面；
2. 由于极值点的全微分为0，故目标函数在最优点$x^{\*}$上的梯度$\nabla f(x^{\*})$正交于约束曲面。

由此可知，在最优点$x^{\*}$上，梯度$\nabla g(x^{\*})$和$\nabla f(x^{\*})$的方向相同或相反，即存在$\lambda \neq 0$使得：

$$
\nabla f(x^{\*}) + \lambda \nabla g(x^{\*}) = 0 \tag {9}
$$

对(9)求原函数得拉格朗日函数：

$$
L(x,\lambda) = f(x) + \lambda g(x) \tag {10}
$$

其中，$\lambda$称为拉格朗日乘子，且原约束优化问题转化为对拉格朗日函数$L(x,\lambda)$的无约束优化问题。即，分别对参数求偏导，令偏导数为$0$即可进行求解。

需要注意的是拉格朗日乘子求的解不一定是最优解，其实只是局部最优解，这里称作可行解，只有在凸函数中才能保证最优解。

## 不等式约束

对于不等式条件$g(x) \leq 0$的情况有两种：即可行解在$g(x)<0$或者$g(x)=0$区域内取得。

在$g(x) \lt 0$内，这时约束条件不起作用。只需要直接对目标函数进行最小化，找在$g(x) \lt 0$的可行解$x^{\*}$，所以有：

$$
\begin{array}{l}
\nabla f(x^{\*}) =0; \\\\\\
\lambda = 0; \\\\\\
g(x^{\*}) \lt 0
\end{array} \tag {11}
$$

在$g(x) = 0$上，类似于上面等式约束的分析，但需要注意的是，此时$\nabla f(x^{\*})$的方向与$\nabla g(x^{\*})$相反，即存在$\lambda \neq 0$，有$\nabla f(x^{\*}) = - \lambda g(x^{\*})$。


整合条件有：

$$
\begin{array}{l}
g(x) \leq 0 \\\\\\
\lambda \geq 0 \\\\\\
\lambda g(x) = 0
\end{array} \tag{12}
$$

(12)式即拉格朗日函数的**`KKT`条件**

以上可以进行推广，考虑具有$m$个等式约束和$n$个不等式约束的非空优化问题：

$$
\begin{array}{ll} 
\underset{x} {min }  & f(x)  \\\\\\
\text{s.t.  }  & h_i(x) = 0 \text{  } (i=1,\cdots,m) \\\\\\
 & g_i(x) \leq 0 \text{  } (j=1,\cdots,n)
\end{array} \tag {13}
$$

引入拉格朗日乘子$\lambda=(\lambda_1,\cdots,\lambda_m)^T$和$\mu = (\mu_1,\cdots,\mu_n)^T$，相应的拉格朗日函数为：

$$
L(x,\lambda,\mu) = f(x) + \sum_{i=1}^{m} \lambda_i h_i(x) + \sum_{j=1}^{n} \mu_j g_j(x) \tag {14}
$$

由不等式约束引入的`KKT`条件($j=1,\cdots,n$)为：

$$
\begin{array}{l}
g_j(x) \leq 0 \\\\\\
\mu_j \geq 0 \\\\\\
\mu_j g_j(x) = 0
\end{array} \tag{15}
$$


## 对偶问题

一个优化问题可以从两个角度来考察，即主问题(`Primal problem`)和对偶问题(`Dual problem`)。对主问题(13)，基于式(14)，其拉格朗日的对偶函数(`Dual Function`)$\Gamma : R^m \times R^n \rightarrow R$定义如下：

$$
\begin{align}
\Gamma (\lambda,\mu) &= \underset{x \in D}{inf } \text{ } L(x,\lambda,\mu) \\\\\\
&= \underset{x \in D}{inf} \text{ } \bigg( f(x) + \sum_{i=1}^{m}\lambda_i h_{i}(x) + \sum_{j=1}^{n}\mu_j g_j(x) \bigg)
\end{align} \tag{16}
$$

由`KKT`条件可知：

$$
\sum_{i=1}^{m}\lambda_i h_{i}(x) + \sum_{j=1}^{n}\mu_j g_j(x) \leq 0 \tag{17}
$$

进而有

$$
\Gamma(\lambda,\mu) = \underset{x \in D}{inf} \text{ } L(x,\lambda,\mu) \leq L(x^{\*},\lambda,\mu) \leq f(x^{\*}) \tag{18}
$$

即对偶函数给出了主问题最优值的下界。显然，主问题的下界取决于$\lambda$和$\mu$的值。于是问题就变成了：基于对偶函数能获得的最好下界是什么？即主问题(13)的对偶问题就成了(19)：

$$
\begin{array}{ll}
\underset{\lambda,\mu}{max} & \Gamma (\lambda,\mu) \\\\\\
s.t. & \mu \geq 0 
\end{array}\tag{19}
$$

## 对偶问题的性质

记主问题为：

$$
\begin{array}{ll}
min & c^T x \\\\\\
s.t. & Ax \leq b,x \geq 0
\end{array}\tag{20}
$$

下面使用拉格朗日乘子法求它的对偶问题。

引入拉格朗日乘子$y = (y_1,\cdots,y_m)^T$，相应的拉格朗日函数为：

$$
L(x,y) = c^Tx + y^T (Ax - b) \tag{21} 
$$

拉格朗日函数对$x$求偏导，并令偏导为0，得：

$$
\frac{\partial L}{\partial x} = c + A^Ty = 0  \Rightarrow c = - A^T y \tag{22}
$$

将(22)代入(21)得：

$$
\Gamma (y) = \text {inf } L(x,y) = - y^TAx + y^T(Ax - b) = y^T b = b^T y \tag{23}
$$

由(21)得`KKT`条件可知：

$$
\begin{array}{l}
y \geq 0 \\\\\\
c - A^Ty \geq 0 \Rightarrow A^T y \leq c
\end{array} \tag{24}
$$

由(18)可知，对偶问题为：

$$
\begin{array}{ll}
max & b^T y \\\\\\
s.t. & A^Ty \leq c,y \geq 0
\end{array}\tag{25}
$$

不太严谨地说，对偶问题可悲看作是原始问题的**行列转置**：

1. 原始问题中的第i行j列系数与其对偶问题中的第j行i列的系数相同；
2. 原始目标函数的各个系数行与其对偶问题右侧的各常系数列相同；
3. 原始问题右侧的各常熟列与其对偶目标函数的各个系数行相同；
4. 在这一对问题中，不等式方向和优化方向相反。

此外，关于对偶问题还有如下基本性质：

1. 对称性：对偶问题的对偶是原问题；
2. 弱对偶性：若$x^{\*}$是原问题的可行解，$y^{\*}$是对偶问题的可行解，则存在$c^T x \leq b^T y$；
3. 无界性：若原问题(对偶问题)为无界解，则其对偶问题(原问题)无可行解；
4. 可行解是最优解时的性质：当$x^{\*}$是原问题的可行解，$y^{\*}$是对偶问题的可行解，当$c^T x^{\*} = b^T y^{\*}$时，$x^{\*},y^{\*}$是最优解；
5. 对偶定理：若原问题有最优解，那么对偶问题也有最优解，且目标函数值相同；
6. 互补松弛性：若$x^{\*},y^{\*}$分别是原问题和对偶问题的最优解，则$y^{\*T} (Ax^{\*} - b) = 0, x^{\*T} (A^Ty^{\*} - c) = 0$

## 对偶问题性质的应用

有如下线性规划问题：

$$
\begin{array}{ll}
min & f(x) = 2x_1 + 3x_2 + 5x_3 + 2x_4 + 3x_5 \\\\\\
s.t. & x_1 + x_2 + 2x_3 + x_4 + 3x_5 \geq 4 \\\\\\
 & 2x_1 - x_2 + 3x_3 +x_4 + x_5 \geq 3 \\\\\\
 & x_j \geq 0, \text{  } j=1,2,\cdots,5
\end{array}
$$

已知其对偶问题的最优解为$y_1^{\*} = \frac{4}{5},y_2^{\*} = \frac{3}{5},\Gamma = 5$，试用对偶理论找出原问题的最优解。

由对偶定理，主问题的最优解$f(x^{\*}) = 5$。

由互补松弛性可知：

$$
\begin{bmatrix}
\frac{4}{5} & \frac{3}{5}
\end{bmatrix} * (
\begin{bmatrix}
1 & 1 & 2 & 1 & 3 \\\\\\
2 & -1 & 3 & 1 & 1
\end{bmatrix} *
\begin{bmatrix}
x_1^{\*} \\\\\\
x_2^{\*} \\\\\\
x_3^{\*} \\\\\\
x_4^{\*} \\\\\\
x_5^{\*}
\end{bmatrix}  -
\begin{bmatrix}
4 \\\\\\
3
\end{bmatrix}
 ) = 0  \tag{a}
$$

$$
\begin{bmatrix}
x_1^{\*} & x_2^{\*} & x_3^{\*} & x_4^{\*} & x_5^{\*}
\end{bmatrix} * (
\begin{bmatrix}
1 & 2 \\\\\\
1 & -1 \\\\\\
2 & 3 \\\\\\
1 & 1 \\\\\\
3 & 1
\end{bmatrix} *
\begin{bmatrix}
\frac{4}{5} \\\\\\
\frac{3}{5}
\end{bmatrix}  -
\begin{bmatrix}
2 \\\\\\
3 \\\\\\
5 \\\\\\
2 \\\\\\
3
\end{bmatrix}
 ) = 0 \tag{b}
$$

由$(b)$得$x_2^{\*} = x_3^{\*} = x_4^{\*} = 0$，代入$(a)$中得

$$
\begin{array}{l}
x_1^{\*} + 3x_5^{\*} = 4 \\\\\\
2x_1^{\*} + x_5^{\*} = 3
\end{array}
$$

解得:

$$
x^{\*} = 
\begin{bmatrix}
1 & 0 & 0 & 0 & 1
\end{bmatrix}^T
$$