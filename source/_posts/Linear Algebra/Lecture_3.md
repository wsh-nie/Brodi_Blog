---
title: "MIT·线性代数课程（三）"
date: 2017-04-08T08:03:02+08:00
draft: false
description: "行列式计算"
tags: 
- courses notes
- linear algebra
categories: 
- linear algebra
mathjax: true
---


# 三个基本性质（定义）和十大性质（定理）
> 1. $det \ I=1$；
> 2. 交换一行，改变正负符号；
> - 3.1 $\begin{vmatrix} ta & tb  \\\\ c & d \end{vmatrix} = t\begin{vmatrix} a & b  \\\\\\ c & d \end{vmatrix}$
> 
> - 3.2 $\begin{vmatrix} a+a^{'} & b+b^{'}  \\\\\\ c & d \end{vmatrix} = \begin{vmatrix} a & b  \\\\\\ c & d \end{vmatrix} + \begin{vmatrix} a^{'} & b^{'}  \\\\\\ c & d \end{vmatrix}$
>4. 如果行列式中有两行相等，那么行列式值为零。
> - 证明：根据性质二，当交换行列式的这两行时，$det=-det$，故知$det=0$。
>5.  减去某一行的`t`倍不改变行列式的值。
> $\begin{vmatrix} a & b  \\\\\\ c-ta & d-tb \end{vmatrix}=\begin{vmatrix} a & b  \\\\\\ c & d \end{vmatrix}-\begin{vmatrix} a & b  \\\\\\ -ta & -ta \end{vmatrix}=\begin{vmatrix} a & b  \\\\\\ c & d \end{vmatrix}-t\begin{vmatrix} a & b  \\\\\\ a & b \end{vmatrix}$

# 代数余子式
# 代数余子式的性质和运用






