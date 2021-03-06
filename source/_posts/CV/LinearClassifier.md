---
title: 线性分类器
draft: false
description: 图像分类，线性机器学习算法
tags:
  - CV
  - AI
categories:
  - CV
mathjax: true
abbrlink: 81dc199e
date: 2020-09-11 14:06:18
---

## 数据集介绍

CIFAR10数据集：包含50000张训练样本、10000张测试样本分为airplane、automobile、bird、cat、deer、dog、frog、horse、ship、truck十个类别图像为彩色图像，大小为32*32。

## 分类器设计

![分类器设计示意图](/images/CV/LinearClassifier_1.png)

## 图像类型

基于像素的图形表示

二进制图像：非黑即白，像素取值范围(0、1)
灰度图像：一个像素取值范围0~255
彩色图像：一个像素取值范围$2^8 * 3$(RGB)


大多数分类算法都要求输入向量，可以直接将像素矩阵按行优先归成一个向量

一个CIFAR10的图像转换为向量是:$32 * 32 * 3 = 3072$ 维

## 分类模型

为什么从线性分类器开始？
1. 形式简单、易于理解
2. 通过层级结构(神经网络)或者高维映射(支持向量机)可以形成功能强大的非线性模型

什么是线性分类器？
线性分类器是一种线性映射，将输入的图像特征映射为类别分数。

### 线性分类器定义：

第i个类的线性分类器：

$$
f_i(x,w_i) = w^T_i x + b_i; \text{  } i = 1,\cdots,c
$$

$x$代表输入的$d$维图像向量，$c$为类别个数，$w_i=[w_{i1},\cdots,w_{id}]^T$为第i个类别的权值向量，$b_i$为偏置。每个类都有自己的参数$w$和$b$。

### 决策规则：

如果$f_i(x) \geq f_j(x), \forall j \not ={i}$，则决策输入图像$x$属于第$i$类。

### 由矩阵规则可得分类向量：

$$
f(x,w) = wx + b
$$

CIFAR10数据集中，对应的x是$3072*1$向量，w为$10 * 3072$矩阵，b是$10 * 1$向量。

### 线性分类器的权值向量：

$w$即是对应类别的模板，记录的是该类别的统计信息。输入图像与评估模板的匹配程度越高，分类器输出的分数就越高。

### 线性分类器的决策边界：

首先线性分类器之所以是线性的就是因为其决策边界是线性的。令$f_i = w^T_i x + b_i =0$即是决策面。$w$控制着线的方向，$b$控制线的偏移。

箭头方向代表分类器的正方向，沿着箭头方向距离决策面越远分数就越高。

## 损失函数

### 损失函数定义

损失韩式搭建了模型性能与模型参数之间的桥梁，指导模型参数优化。

损失函数是一个函数，用于度量给定分类器的预测值与真实值的不一致程度，其输出通常是一个非负实值。

其输出的非负实值可以作为反馈信号来对分类器参数进行调整，以降低当前实例对应的损失值，提升分类器的分类效果。

损失函数的一般定义：

$$
L = \frac{1}{N} \sum_i L_i(f(x_i,W),y_i)
$$

$L$为数据集损失，它是数据集中所有样本损失的平均。

如果初始化时w和b很小，损失函数L会如何？可以用于检验编码是否出现错误。

### 多类支持向量机损失

$$
s_{ij} = f_j(x_i,w_j,b_j) = w^T_j x_i + b_j
$$

$s_{ij}$为第$i$个样例在第$j$类分类上的得分。

第i个样本的多类支持向量机损失定义如下：

$$
\begin{align}
L_i &= \sum_{j \not ={y_i}} \begin{cases} 0 & \text{if } s_{y_i} \geq s_{ij} + 1 \\\\\\ s_{ij} - s_{y_i} + 1 & \text{otherwise} \end{cases} \\\\\\
&= \sum_{j \not ={y_i}} \text{max}(0, s_{ij} + s_{y_i} + 1)
\end{align}
$$

$s_{y_i}$表示第$i$个样本真实类别的预测分数，$+1$是为了偏离边界，边界状态介于两个类别之间，不稳定。

正确类别的得分比不正确类别的得分高出1分，就没有损失；否则就会产生损失。

### 正则项与超参数

假设存在一个$W$使损失函数$L=0$，这个$W$是唯一的吗？如何选择一个很好的$W$？

所以对损失函数引入一个正则项：

$$
L(W) = \frac{i}{N} \sum_{i} L_i(f(x_i,W),y_i) + \lambda R(W)
$$

数据损失：模型预测需要和训练集相匹配；
正则项损失：防止模型在训练集上学习得“太好”。

这就能使得损失函数为$0$时只有唯一的$W$。

$R(W)$是一个与权值有关，跟图像数据无关的函数；$\lambda$是一个超参数控制着正则损失在总损失中所占的比重。

什么是超参数？在学习之前设置而不是在学习中获得的参数。超参数一般的都会对模型性能有着重要的影响。

L2正则项：$R(W) = \sum_{k} \sum_{i} W^2_{k,i}$

L2正则损失对大数值权值进行惩罚，喜欢分散权值，鼓励分类器将所有维度的特征都用起来，而不是强烈的依赖其中少数几维特征。正则项让模型有了偏好。在这个意义上，正则项有三个好处，一是使得$W$的解唯一，二是使得模型的选择有偏好，三是防止过拟合。

## 优化算法

### 什么是优化

参数优化是机器学习的核心步骤之一，它利用损失函数的输出值作为反馈信号来调整分类器参数，以提升分类器对训练样本的预测幸能。

### 优化算法目标

损失函数$L$是一个与参数$W$有关的函数，优化的目标就是找到使损失函数$L$达到最优的那组参数$W$。

直接方法：

$$
\frac{\partial L}{\partial W} = 0
$$

然而在实际中，L形式比较复杂，很难从这个等式直接求解出W。

### 梯度下降算法

梯度下降：利用所有样本计算损失并更新梯度

> while True
    权值的梯度 <== 计算梯度(损失，训练样本，权值)
    权值 <== 权值 - 学习率 * 权值梯度

### 梯度计算

1.数值法

一维变量，函数求导：

$$
\frac{d L(w)}{w} = \lim_{h->0} \frac{L(w+h)-L(w)}{h}
$$

计算量大，不精确

2.解析法

先求导后代入计算。

优点：精确，速度快
缺点：导数函数推导易错