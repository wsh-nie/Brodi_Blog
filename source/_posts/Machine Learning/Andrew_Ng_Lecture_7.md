---
title: "吴恩达·机器学习课程（七）"
date: 2020-05-25 14:00:25
draft: false
description: "第七周学习内容：支持向量机"
tags: 
- courses notes
- machine learning
categories: 
- machine learning
mathjax: true
abbrlink: dd767d1a
---

# 支持向量机(`Supprot Vector Machines`)

对于监督学习的分类问题，已经学习过逻辑回归和神经网络两种算法。今天将介绍一个更加强大的算法，即支持向量机。与前两个算法相比，支持向量机在学习复杂的非线性方程式提供了一种更为清晰，更加强大的方式。

## 优化目标

事实上可以对逻辑回归的优化来展示如何一步步修改得到本质上的支持向量机。

在逻辑回归中，假设函数为$h_{\theta}(x) = \frac{1}{1+e^{\theta^Tx}}$。

在逻辑回归中，在训练集或者交叉验证集中，有一个y=1样本的样本，我们希望$h_{\theta}(x) = \frac{1}{1+e^{\theta^Tx}}能够尽可能地趋近于1，根据sigmoid函数的性质，即需要\theta^T x远大于0；相反地，如果我们有另一个样本，即y=0。我们希望假设函数的输出值将趋近于0，这对应于\theta^Tx$远小于0，因为对应的假设函数的输出值趋近0。

转换视角到逻辑回归的代价函数上，可以知道的是每一个训练集样例都会为总的代价函数有所贡献，即增加$-(y \text{ log } h_{\theta}(x) + (1-y)\text{ log }(1-h_{\theta}(x))$，将预测函数代入其中即:

$$
-y \text{ log } \frac{1}{1+e^{-\theta^Tx}} - (1-y)\text{ log }(1-\frac{1}{1+e^{-\theta^Tx}})
$$

当y=1时，样本的代价函数如下图左，当$\theta^Tx非常大时，函数值变得非常小，即对总的代价的贡献非常小。所以在构建支持向量机的时候，就需要对函数进行一定的修正为新的代价函数(Cost_1(z))，在z=1之后部分设置为0，z=1之前的用直线近似，不需要考虑斜率的问题，毫无疑问这个近似的代价函数跟逻辑回归的代价函数功能上相差无几，实际上支持向量机拥有计算上的优势，并且能够使得优化问题变得简单。当y=0时，如下图右，使用同样的方法去近似逻辑回归的代价函数构建新的代价函数(Cost_0(z)$)。

![图1](/images/Machine_Learning_Lecture7_1.png)

根据上面的定义，对于逻辑回归的代价函数同样进行相同的修改。

$$
\text{min } \frac{1}{m} \bigg [ \sum_{i=1}^{m} y^{(i)}(- \text{log } h_{\theta}(x^{(i)}) + (1-y^{(i)}) (-\text{log }(1-h_{\theta}(x^{(i)}))) \bigg ] + \frac{1}{2m} \sum_{j=1}^{n} \theta_{j}^2
$$

$$
\text{min } \frac{1}{m} \bigg [ \sum_{i=1}^{m} y^{(i)} Cost_1(\theta^Tx^{(i)}) + (1-y^{(i)}) Cost_0(\theta^Tx^{(i)}) \bigg ] + \frac{1}{2m} \sum_{j=1}^{n} \theta_{j}^2
$$

由于整个优化对象的常系数不影响优化，首先可以去掉常系数$\frac{1}{m}，优化对象可以简化为A+\lambda B。可以通过设定不同的正则化参数\lambda$以便能够权衡我们想在多大程度上去适应训练集。

而对于SVM，使用不同的参数，即总体优化目标就成了：

$$
\text{min } C \bigg [ \sum_{i=1}^{m} y^{(i)} Cost_1(\theta^Tx^{(i)}) + (1-y^{(i)}) Cost_0(\theta^Tx^{(i)}) \bigg ] + \frac{1}{2} \sum_{j=1}^{n} \theta_{j}^2
$$

此外，SVM的假设函数输出不是概率，而是预测准确值，即：

$$
h_{\theta}(x)=
\begin{cases}
1, & \theta^Tx \geq 0 \\\\\\
0, & \text{otherwise}
\end{cases}
$$

## 大边界的直观理解

支持向量机也被称为大间距分类器(`Large Margin Classifiers`)，接下来将展开理解为什么说SVM是大间距分类器。

如下图，考虑最小化代价函数的必要条件，如果有一个正样本y=1，则只有在$\theta^T x \geq 1时，代价函数才为0，换而言之当有一个正样本，优化的目标就是\theta^T x \geq 1。同样对于负样本，优化的目标是\theta^T x \leq -1$

![图2](/images/Machine_Learning_Lecture7_2.png)

事实上，对于一个正样本,其实我们只需要使$\theta^Tx \geq 0，就能正确分类。类似地，对于负样本，则仅需要\theta^Tx \leq 0$就会将负例正确分离。

而支持向量机的要求更高，这就相当于在支持向量机中嵌入了一个额外的安全因子，或者说安全的间距因子。

我们考虑，如果将SVM的代价函数常数C设置为很大，即对于整个优化目标，希望每个样本的误差均为0。即优化的重点放在左半部分，而可以"忽略"右边的$\frac{1}{2} \sum_{j=1}^{n} \theta_j^2$。

即对于正样本，希望$\theta^Tx \geq 1，这样能够使得样本误差为0；同样对于负样本，希望\theta^Tx \leq -1。然而实际上对于正样本只要\theta^Tx \geq 0，对于负样本只需要\theta^Tx \leq 0$，就可以准确分类，那么追求SVM的最优化目的何在？

对于分类问题，最基本的思想就是基于训练集在样本空间中找出一个**划分超平面**，将不同类别的样本分开。如下图，划分样本空间的超平面有很多，我们应该选择哪一个呢？

![图3](/images/Machine_Learning_Lecture7_3.png)

直观上而言，我们喜欢中间的黑色划分超平面，除却能够满足向量机方程，具有和总体样本间最大的间距，也具有良好的鲁棒性。SVM即致力于针对样本空间找出这样的划分超平面，这也是支持向量机也被称为大间距分类器的缘由。

如下图，当C被设置的非常大时，支持向量机得出了黑色的决策边界。当增加一个新的样本时，为了满足支持向量机方程式，则决策边界变成了粉色。

![图4](/images/Machine_Learning_Lecture7_4.png)

显然，仅仅一个样本导致决策边界发生大幅度改变是不明智的，在这里可以将原本很大的C变小，这时优化的对象就不能再"忽略"右边部分了，故SVM会忽略掉异常点的影响，即决策边界还是黑色超平面。

## 大边界分类的数学原理

如下图，向量的内积一方面可以用投影和L2范数来表示，需要注意的是，当两向量夹角超过直角，则内积值为负。

![图5](/images/Machine_Learning_Lecture7_5.png)

对于SVM的优化目标函数：

$$\begin{align}
& \text{min } \frac{1}{2} \sum_{j=1}^{n} \theta_j^2 & \\\\\\
\text{s.t.   } & \theta^Tx^{(i)} \geq 1 & \text{if } y^{(i)} = 1 \\\\\\
 & \theta^Tx^{(i)} \leq -1 & \text{if } y^{(i)} = 0 
\end{align}
$$

在这里，令$\theta_0 = 0，则可知\frac{1}{2} \sum_{j=1}^{n} \theta_j^2 = \frac{1}{2} || \theta ||^2$

由向量内积可知：

$$
\theta^Tx^{(i)} \geq 1 \Rightarrow p^{(i)} ||\theta|| \geq 1
$$

则优化目标可以表示为：

$$
\begin{align}
& \text{min } \frac{1}{2} || \theta ||^2 & \\\\\\
\text{s.t.   } & p^{(i)} ||\theta|| \geq 1 & \text{if } y^{(i)} = 1 \\\\\\
 & p^{(i)} ||\theta|| \leq -1 & \text{if } y^{(i)} = 0 
\end{align}
$$

即需要是的x(i)到$\theta所表示的超平面间距p^{(i)}$尽可能的大。

## 核函数1

这个章节将改进支持向量机算法来实现复杂的非线性分类器。主要使用的就是称之为核(`Kernel`)的东西。

针对无法用直线进行分割的分类问题，需要使用的高级多项式模型，即为了获得如图的决策边界，需要的模型可能是$\theta_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_1x_2 + \theta_4 x_1^2 + \theta_5 x_2^2 + \cdots$。

![图6](/images/Machine_Learning_Lecture7_6.png)

使用一组新的特征f来替换模型中的多项式，得：$\theta_0 + \theta_1 f_1 + \theta_2 f_2 + \theta_3 f_3 + \cdots$。

思考，除了对原有的特征进行高幂次的组合意外，有没有更好的方法来构建特征量？

核方法即是用于构建特征量的。

给定一个训练样本x，基于x和手动选择的一些**地标**(`landmarks`)l(1),l(2),l(3)的近似程度来生成新的特征f1,f2,f3：

$$
\begin{align}
f_1 = \text{similarity}(x,l^{(1)}) = exp \big( - \frac{||x-l^{(1)}||^2}{2 \sigma^2} \big) \\\\\\
f_2 = \text{similarity}(x,l^{(2)}) = exp \big( - \frac{||x-l^{(2)}||^2}{2 \sigma^2} \big) \\\\\\
f_3 = \text{similarity}(x,l^{(3)}) = exp \big( - \frac{||x-l^{(3)}||^2}{2 \sigma^2} \big) \\\\\\
\end{align}
$$

上面的$\text{similarity}$函数就是核函数，具体而言，这里是高斯核函数(`Gaussian Kernel`)，需要注意的是这个函数和正态分布没有什么实际上的联系。

这些地标又有什么作用呢？

如果一个训练样本x与地表l之间的距离近似0，则新特征f近似于e−0=1，如果训练样本x与地标之间距离较远，则f近似于$e^{- \infty} = 0$。

假设我们的训练样本含有两个特征x1,x2，给定地标l(1)与不同的$\sigma$值，见下图：

![图7](/images/Machine_Learning_Lecture7_7.png)

图中水平面的坐标为x1,x2而垂直坐标轴代表f。

可以看出，只有当x与l(1)重合时f才具有最大值。随着x的改变f值改变的速率受到$\sigma^2$的控制。

在下图中，当样本处于蓝色点x位置处，因为其离l(1)更近，但是离l(2)和l(3)较远，因此f1接近1，而f2,f3接近0。因此hθ(x)=θ0+θ1f1+θ2f2+θ1f3>0，因此预测y=1。同理可以求出，对于离l(2)较近的黑色点，也预测y=1，但是对于圈外的点，因为其离三个地标都较远，预测y=0。

![图8](/images/Machine_Learning_Lecture7_8.png)

这样，图中封闭曲线所表示的范围，便是我们依据一个单一的训练样本和我们选取的地标所得出的判定边界。

在预测时，我们采用的特征不是训练样本本身的特征，而是通过核函数计算出的新特征f1,f2,f3。

## 核函数2



## 使用支持向量机