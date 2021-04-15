---
layout: post
title: "MIT·线性代数课程（一）"
date: 2017-04-04 08:03:02
draft: false
description: "消元法"
tags: 
- courses notes
- linear algebra
categories: 
- linear algebra
mathjax: true
abbrlink: 9a70ea35
---

# 为什么需要进行消元法

## 1.矩阵表示
给定一个多元一次方程组， 可以用矩阵的形式直接抽象表示出来。对于一个`m`个方程`n`个未知元的方程组可以直接用一个`m*n`的矩阵A左乘一个未知元向量x表示，然后用以向量b表示。

$$
\begin{cases}
\begin{aligned} 
x_1+2x_2+x_3 &=2 \\\\\\
3x_1+8x_2+x_3 &=12\\\\\\
4x_2+x_3 &=2 
\end{aligned} 
\end{cases}
$$

即，可以表示成:
Ax=b

$$\begin{bmatrix}1 & 2 & 1 \\\\\\
3 & 8 & 1 \\\\\\
0 & 4 & 1 
\end{bmatrix}*
\begin{bmatrix}x1 \\\\\\
x2 \\\\\\
x3 
\end{bmatrix}
 = \begin{bmatrix}2 \\\\\\
 12 \\\\\\
 2 
  \end{bmatrix}$$

## 2.为什么需要消元

上面的矩阵表示方法虽然能够抽象的表示方程组，但是对于如何求解方程组来说还是没有任何办法。 最理想的能求解方程是一元一次方程。所以消元法就来了。消元法 能够将方程的矩阵形式化简成一个上三角矩阵(`Upper-triangular`)记作U。 通过U矩阵就能对向量x尝试求解操作了。

# 如何进行消元
整体来说就是将矩阵A进行一些列的初等变化将A化成一个上三角矩阵，具体的操作有行交换、行消元。
>1.选择主元(每行一个)。确定当前列，通过行交换找到当前行以下一个非零元素。
2.把当前行以下的在这个列上的元素全部消为`0`。
这里 仍然拿上面的矩阵举例子。
$$\begin{bmatrix}1. & 2 & 1 \\\\\\ 3 & 8 & 1 \\\\\\ 0 & 4 & 1  \end{bmatrix} \Rightarrow 
\begin{bmatrix}1. & 2 & 1 \\\\\\ 0 & 2. & -2 \\\\\\ 0 & 4 & 1  \end{bmatrix} 
\Rightarrow 
\begin{bmatrix}1. & 2 & 1 \\\\\\ 0 & 2. & -2 \\\\\\ 0 & 0 & 5. \end{bmatrix} 
$$

如上面操作，第一行主元选择第一列，非零不需要行交换操作。然后将第二行和第三行的第一列元素通过相减消元全部化成0。后面的一次进行。

$$U=\begin{bmatrix}1 & 2 & 1 \\\\\\ 0 & 2 & -2 \\\\\\ 0 & 0 & 1 \end{bmatrix} $$

>这里 使用简单的初等变换(`Elementary Operation`)消元，可以直接对矩阵A左乘变换矩阵E来操作矩阵A。下面直接使用左乘变换矩阵。
注意这里 没有进行任何行交换(`raw exchange`)
1. 第一步，选定第一行主元，第二行减去第一行的两倍。
   
$$E_{21}=\begin{bmatrix}1 & 0 & 0 \\\\\\ -2& 1 & 0 \\\\\\ 0 & 0 & 1  \end{bmatrix} $$

$$\begin{bmatrix}1 & 0 & 0 \\\\\\ -2& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix} \times \begin{bmatrix}1 & 2 & 1 \\\\\\ 3 & 8 & 1 \\\\\\ 0 & 4 & 1 \end{bmatrix} =\begin{bmatrix}1& 2 & 1 \\\\\\ 0 & 2 & -2 \\\\\\ 0 & 4 & 1 \end{bmatrix} $$

2. 第二步，第三行对应的第一列不需要操作，左乘不变换

$$E_{31}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix} $$

3. 第三步，选择第二行主元，第三行减去第二行的两倍

$$E_{32}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & -2 & 1 \end{bmatrix} $$

$$\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & -2 & 1 \end{bmatrix} \times \begin{bmatrix}1& 2 & 1 \\\\\\ 0 & 2 & -2 \\\\\\ 0 & 4 & 1 \end{bmatrix}=
\begin{bmatrix}1& 2 & 1 \\\\\\ 0 & 2& -2 \\\\\\ 0 & 0 & 5 \end{bmatrix}  $$

$$E = E_{32} \times E_{31} \times E_{21}$$

$$E \times A=U$$

观察到U矩阵右下角， 得到了 喜欢的一元一次方程了，可以开始求解了。然后依次迭代可以求出啦。当然，这里一定要注意同时对向量b进行一样的初等变换。
$$
\left[\begin{array}{ccc|c}1 & 2 & 1 &2 \\\\\\ 3 & 8 & 1 &12 \\\\\\ 0 & 4 & 1 &2 \end{array} \right]\Rightarrow 
\left[\begin{array}{ccc|c}1 & 2 & 1 &2 \\\\\\ 0 & 2 & -2 &6 \\\\\\ 0 & 4 & 1 &2 \end{array} \right]
\Rightarrow 
\left[\begin{array}{ccc|c}1 & 2 & 1 &2 \\\\\\ 0 & 2 & -2 &6 \\\\\\ 0 & 0 & 5 &-10 \end{array} \right]
$$

$$X=\begin{bmatrix}2 \\\\\\ 1 \\\\\\ -2 \end{bmatrix} $$

# U矩阵里面蕴含了什么呢？

通过消元， 能够将矩阵$A_{m \times n}化简成上三角矩阵U。然后可以开始尝试求解方程组的解了。这里为什么说是尝试，因为就算化简到U矩阵，也不一定能确定方程的解。可以这样说，通过消元法将矩阵A化成U$之后， 能判断一个矩阵的好坏。当 得到`r`个主元(`pivot`)。即Rank(A)=r.也就是矩阵的**秩(Rank)**了。 还能得到另外一个定义：自由变元(`free variable`)(f=n−r)。

 化简得到的U矩阵(`Upper-triangular`)。 将每个主元化简为`1`，这里只要对每一行乘以主元的倒数，然后对每一列消去所有主元元素。 得到的是R(`Reduced raw echelon from`)矩阵。这个操作同样是初等变换，不改变主元个数，不改变方程组的解。 注意到这个操作会使得所有的主元所在列除主元为`1`以外，所有的元素都是`0`。如果 将这个主元列进行列交换(注意，此前没有进行过任何列交换) 可以得到如下矩阵：

$$R=\begin{bmatrix} I & F \\\\\\ 0 & 0 \end{bmatrix}$$

F矩阵是自由变元组成。 知道一旦有了变元，则方程就有了无穷解的可能了。

 知道这个秩 r<=min(m,n) ;下面对这个秩和方程组的解情况进行一下讨论。

>1.r=m=n(满秩)，方程组有唯一解.  
>
>2.r=m<n(行满秩)，方程组有唯一解或者无穷解.  
>
>3.r=n<m(列满秩)，方程组无解或者有无穷解；  
>
>4.r<m,r<n， 方程组无解或者无穷解；  
上面总结了有了自由变元矩阵，方程组就有了无穷解的可能。这里再总结方程无解的情况。对于m>r时，存在向量b对应的大于`r`的位置的量非零，还原方程组即存在非零等于零的情况。这个下次详解。

# 通过消元能得到的结果

上面显示 可以通话消元法化简A到U然后方便求解解空间X。同样的 还可以通过消元法求解一个矩阵的逆元(当逆元存在的时候)。  

 知道，消元法对矩阵进行操作， 可以转换成左乘一个变换矩阵E来进行消元。

首先 先清楚定义**逆元(inverse)**

>当方形矩阵满足 A−1A=I (`Identity`) 称A−1和A互为逆元。
根据运算规则 AA−1=I同样成立。

简单证明：
$$(A^{-1}A)^{-1}=I^{-1}\Rightarrow (A^{-1})^{-1}A^{-1}=I^{-1}\Rightarrow AA^{-1}=I^{-1}=I.$$

在这里再构造如下矩阵进行消元可得矩阵A的逆。

$$\begin{bmatrix} A & I \end{bmatrix} $$

矩阵A左乘一个变换矩阵E就能对A进行初等变换，这里就是通过将A进行消元变换到单位矩阵I
这里的消元同化简得到R矩阵一样，需要使得每一列非主元元素为`0`。

$$E * \begin{bmatrix} A & I \end{bmatrix}=\begin{bmatrix}I&E \end{bmatrix} 
此时 E = A^{-1}\Rightarrow$$

$$E*\begin{bmatrix}A&I \end{bmatrix}=\begin{bmatrix}EA & EI\end{bmatrix}=\begin{bmatrix}I&E \end{bmatrix} 	\Rightarrow EA=I \Rightarrow E=A^{-1}$$

对于逆元这里有一个有趣的结论，可以将A经过消元(初等变换)得到U。那么 如何从U经过还原(初等变换)得到A呢？当然是撤回操作啦。

$$E_{32} \times E_{31} \times E_{21} \times A = U$$

由逆元可知，只要在等式两面左乘`E`的逆元就好了。

$$E^{-1} \times E \times A=E^{-1} \times U=A$$

$$E_{21}^{-1} \times E_{31}^{-1} \times E_{32}^{-1} \times (E_{32} \times E_{31} \times E_{21}) \times A=E^{-1} \times U=A$$

记L(`Lower-triangular`)

L=E−1

$$A=L \times U$$

有趣的事就要来了，就上面的例子分别计算出E以及其逆元吧。

$$E_{21}=\begin{bmatrix}1 & 0 & 0 \\\\\\ -2& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix}, E_{21}^{-1}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 2& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix}$$

$$E_{31}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix} ,E_{31}^{-1}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix}$$

$$E_{32}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & -2 & 1 \end{bmatrix},E_{32}^{-1}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & 2 & 1 \end{bmatrix}  $$

$$E= \begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & -2 & 1 \end{bmatrix}*\begin{bmatrix}1 & 0 & 0 \\\\\\ -2& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix}=\begin{bmatrix}1 & 0 & 0 \\\\\\ -2& 1 & 0 \\\\\\ 4.& -2 & 1 \end{bmatrix}$$

$$E^{-1}= \begin{bmatrix}1 & 0 & 0 \\\\\\ 2& 1 & 0 \\\\\\ 0 & 0 & 1 \end{bmatrix}*\begin{bmatrix}1 & 0 & 0 \\\\\\ 0& 1 & 0 \\\\\\ 0 & 2 & 1 \end{bmatrix}=\begin{bmatrix}1 & 0 & 0 \\\\\\ 2.& 1 & 0 \\\\\\ 0 &2. & 1 \end{bmatrix}$$

也就是说，知道了A的每次初等变换矩阵， 就能直接写出L矩阵。(注意矩阵中标记元素)