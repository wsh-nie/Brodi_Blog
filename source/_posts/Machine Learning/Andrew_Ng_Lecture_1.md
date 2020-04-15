---
title: "吴恩达·机器学习课程（一）"
date: 2020-04-04T08:03:02+08:00
draft: false
description: "第一周学习内容：什么是机器学习？机器学习的分类，以及梯度下降算法"
tags: 
- courses notes
- machine learning
categories: 
- machine learning
mathjax: true
---

# 什么是机器学习

## 两种定义  

1. 无需准确编程就能使计算机获得学习能力的研究领域  

>`Arthur Samuel`：the field of study that gives computers the ability to learn without being explicitly programmed.  

2. 一个计算机程序学习关于某类任务T的经验E，其中T的性能表现由P进行测量，如果这个程序在任务T上表现被P测量为提升，即(该程序)学习了经验E

>`Tom Mitchell`：A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E.  

**Example**: *playing checkers*.  
**E** = `the experience of playing many games of checkers`  
**T** = `the task of playing checkers`  
**P** = `the probability that the program will win the next game`  

## 运用领域  

1. 数据挖掘`Database Mining`  
   
    网页点击数据，医疗记录，生物学，工程学等  
2. 无法人工编程的应用  
   
    自动驾驶，手写识别，NLP，计算机视觉等  
3. 自定式程序  
   
    Amazon的推荐系统等  
4. 理解人类学习方式  

> 机器学习在十二项IT技术人员必备技能中排第一  

# 机器学习算法  

## 分类  

1. 监督式学习`Supervised learning`  
    包括：回归`Regression`和分类`Classification` 

2. 非监督式学习`Unsupervised learning`  
    包括：聚类`Clustering`  

3. 强化学习`Reinforcement learning`  

4. 推荐系统`Recommender systems`

## 回归算法

监督式学习算法中能对每个数据样例，能够给出确定的回答。能够预测**准确数值**`real-value`的称之为回归，预测**离散值**`discrete-value`的称之为分类.  

对于一个训练数集，有如下概念：  
**m** = ·`Number of training examples`  训练集大小  
**x** ’s = `"input" variable/features` 输入变量或特征  
**y** ’s = `"output" variable/"target" variable` 输出变量或目标变量  


机器学习算法的流程如下，先通过对训练集的学习获得假设函数`hypothesis`，然后通过假设函数来获得需要的值。  

<div class="mermaid">
    graph TB
        subgraph 流程
        I(Input) --> H([Hypothesis])
        H([Hypothesis]) --> O(Output)
            subgraph 学习获得函数h
            D[Training Set] --> H([Hypothesis])
            end
        end
</div>
  
# 模型表示——一元线性回归

## 模型

一元线性回归又称之为**单变量线性回归**，是最简单最基本的一中回归模型，其训练的数据集中含有两个变量，即`x`和`y`均只有一个变量。  

所以其能够建立的模型函数为：
$$
h\_\theta (x) = \theta\_0 + \theta\_1 x
$$

我们的目标就是选择合适的$\theta\_0$和$\theta\_1$来使得$h\_\theta$接近于训练集中样例中的$y$

## 代价函数

代价函数是用于评判模型合理性的一个指标，代价函数值越小表示训练集获得假设函数用于对我们给出的`x`参数估计`y`值时越精确。  

对于一元线性回归模型，我们的代价函数可以表示为如下：  

$$
J(\theta\_0,\theta\_1) = \frac{1}{2m} \sum\_{i=1}^{m}(h\_\theta(x^{(i)}) - y^{(i)})^{2} = \frac{1}{2m} \sum\_{i=1}^{m}((\theta\_0+\theta\_1 x) - y^{(i)})^{2}
$$

其中，$(x^{(i)},y^{(i)})$是训练集中的样例，$J(\theta\_0,\theta\_1)$就是对模型中两个参数进行评价的代价函数。  

我们的**目标**就是让代价函数最小化：$minimize \  J(\theta\_0,\theta\_1)$ 


# 梯度下降算法

当我们通过不同的$\theta\_0$和$\theta\_1$获得大量的代价函数$J(\theta\_0,\theta\_1)$，我们该如何找到最小的代价函数呢？  

梯度下降算法就是用于解决这个问题  
## 算法介绍
梯度下降算法的大体流程如下：  
> 选择某些$\theta\_0$和$\theta\_1$开始，通过不断地改变$\theta\_0$和$\theta\_1$的值来减小代价函数$J(\theta\_0,\theta\_1)$的值，直到我们找到最小值。  

用伪代码描述为：
> repeat until convergence {  
>  
>    $\theta\_j := \theta\_j - \alpha \frac{\partial}{\partial \theta\_j} J(\theta\_0,\theta\_1)$  (for j = 0 and j = 1)  
>  
> }  

其中$\alpha$我们称之为学习速率`learning rate`

这里需要**特别注意**的是，$\theta\_0$和$\theta\_1$两个变量是更新是需要**同步**`simultaneously`的，如下：  

> $temp0 := \theta\_0 - \alpha \frac{\partial}{\partial \theta\_0} J(\theta\_0,\theta\_1)$  
>  
> $temp1 := \theta\_1 - \alpha \frac{\partial}{\partial \theta\_1} J(\theta\_0,\theta\_1)$  
>  
> $\theta\_0 := temp0$  
>  
> $\theta\_1 := temp1$  

## 梯度下降算法应用于线性回归

计算两个偏导数：  
$$
\frac {\partial}{\partial \theta\_0} J(\theta\_0,\theta\_1) = \frac {1}{m} \sum\_{i=1}^{m} (h\_{\theta}(x^{i}) - y^{i})
$$
$$
\frac {\partial}{\partial \theta\_1} J(\theta\_0,\theta\_1) = \frac {1}{m} \sum\_{i=1}^{m} (h\_{\theta}(x^{i}) - y^{i}) \cdot x^{i}  
$$

所以，在一元线性回归问题中，梯度下降算法伪代码如下：  
> repeat until convergence{  
>  
>    $\theta\_0 := \theta\_j - \alpha \frac {1}{m} \sum\_{i=1}^{m} (h\_{\theta}(x^{i}) - y^{i})$  
>  
>    $\theta\_1 := \theta\_j - \alpha \frac {1}{m} \sum\_{i=1}^{m} (h\_{\theta}(x^{i}) - y^{i}) \cdot x^{i}$  
>  
> } 

由上可知，当偏导数(即梯度)为`0`时，我们的循环即终止。  

两个参数每次的改变大小与$\alpha$相关，而且当$\alpha$**太小**，梯度下降会太慢，而当$\alpha$**太大**时，则梯度下降会超过最小值，它可能无法收敛，甚至发散  

由于梯度下降算法实际上是由于梯度为`0`而收敛的，当我们的代价函数具有多个驻点时，梯度下降算法只能求得局部最优解`local optima`  

此外，每次执行算法中不需要修改$\alpha$的值，梯度下降算法能够收敛于局部最优解。当我们接近局部最小值时，梯度下降将自动采取较小的步骤