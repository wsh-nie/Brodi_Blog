---
layout: post
title: 吴恩达·机器学习课程（四）
draft: false
description: 第四周学习内容：神经网络简述与样例介绍
tags:
  - courses notes
  - machine learning
categories:
  - machine learning
mathjax: true
abbrlink: 2dcc1a34
date: 2020-04-18 00:46:37
---

# 神经网路：表示

## 非线性假设

为什么需要引入神经网络？

在之前的学习中，无论是线性回归还是逻辑回归，当特征量太多，计算的负荷会变得非常大，算法的效率也就变得非常低。并且特征量之间的组合会使得问题变得更加复杂。

就计算机视觉方面而言，假设一张图片的大小是$50 * 50px$，并且只选用灰度图片，那么一个样例有$50 * 50=2500$个特征量。当进一步选择图片上两个不同位置上的两个像素当作特征量，那么将有$\frac{2500^2}{2} \approx 3000000$个特征量。

逻辑回归并不适用学习复杂的非线性假设，也就是当$n$非常大，需要学习新的技术——神经网络。

## 神经元和大脑

神经网络并不是新兴的一门技术，早期的人工智能领域就有学者指出应该靠模拟和学习人的学习方式来让机器拥有“智能”。随着现代计算机计算能力的提升，才足以运行大规模的神经网络。

对人类处理外界信息而言，首先通过感官对外界进行感知作为输入，然后神经中枢对感官进行计算处理，最后生成输出。

现在已经有大量的工业产品是通过选择另外的器官代替有输入障碍的人士来让他们真实的感知到这个世界。

比如将传感器放在舌头上，就可以把舌头对物体的感知作为输入传递到视觉处理中枢，让盲人可以看到这个世界的样子。还可以通过触觉皮带来帮助盲人感知方位。

## 模型表示

在人类大脑中，每一个神经元都可以被认为是一个处理单元(`Processing Unit`)，它含有的多个轴突(`Dendrite`)可以当作输入，轴突(`Axon`)可当作输出。

在神经网络之中，每一个神经元又都是一个个学习模型。这些神经元采纳一些特征作为输入，又根据自身模型产生一个输出。

根据以上，设计出神经网络模型，如下图：

![图1](/images/Machine_Learning_Lecture4_1.png)

第一层是输入层(`Input Layer`)，$x_1$，$x_2$，$x_3$是输入单元(`input unit`)，即原始数据。第二层是隐藏层(`Hidden Layers`)，$a_1^{(2)}$，$a_2^{(2)}$，$a_3^{(2)}$是中间单元，负责对上一层的数据进行处理，并且传递到下一层，最后一层是输出层(`Output Layer`)，负责计算$h_{\Theta}(x)$。注意，除第一层和最后一层，所有中间层都称之为隐藏层。并且每一层都增加一个偏差单元(`bias unit`)$a_0^{(j)}$。

注意，$a_i^{(j)}$表示第j层的第i个激活单元(`activation unit`)，$\Theta^{(j)}$表示第$j$层到第$j+1$层的控制函数参数矩阵。

上述模型，激活单元和输出可分别表示为：

$$\begin{matrix}
a_1^{(2)} = g( \Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3) \\\\\\
a_2^{(2)} = g( \Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_1 + \Theta_{22}^{(1)}x_2 + \Theta_{23}^{(1)}x_3) \\\\\\
a_3^{(2)} = g( \Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_1 + \Theta_{32}^{(1)}x_2 + \Theta_{33}^{(1)}x_3) \\\\\\
h_{\Theta}(x)=a_1^{(3)} = g( \Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)})
\end{matrix}
$$

将第$j$层到第$j+1$层用矩阵表示为：

$$
a^{(j+1)} = g(\Theta^{(j)} a^{(j)})
$$

其实，神经网络就像是多层次的逻辑回归。可以把$a_0,a_1,a_2,a_3$看成更为高级的特征量，也就是$x_0,x_1,x_2,x_3$的进化体，并且他们是由$x$与$\Theta$决定的，因为是梯度下降的，所以$a$是变化的，并且变得越来越厉害，所以这些更高级的特征量远比仅仅将$x$高次幂做特征量能更好的预测新数据。这也是神经网络相比于逻辑回归和线性回归的优势。

## 例子和直觉理解

从本质上讲，神经网络能够通过学习得出其自身的一系列特征。在普通的逻辑回归中，被限制为使用数据中的原始特征$x_1,x_2,...,x_{n}$，虽然可以使用一些二项式项来组合这些特征，但是仍然受到这些原始特征的限制。

在神经网络中，原始特征只是输入层，在上面三层的神经网络例子中，第三层也就是输出层做出的预测利用的是第二层的特征，而非输入层中的原始特征，可以认为第二层中的特征是神经网络通过学习后自己得出的一系列用于预测输出变量的新特征。

下面从构造逻辑运算来理解神经网络的厉害之处。

首先，使用一个单层神经元来表示逻辑运算的与运算($AND$)和或运算($OR$)。

![图2](/images/Machine_Learning_Lecture4_2.png)

如上图，$x_1,x_2 \in \lbrace 0,1 \rbrace$，通过对不同的学习获得不同的$\Theta$就可以获得对应的假设函数。

假设通过学习$AND$运算训练集获得$\Theta = \begin{bmatrix} -30 \\\\\\ 20 \\\\\\ 20 \end{bmatrix}$。

所以可以得出$h_{\Theta}(x) = g(-30 + 20 x_1 + 20 x_2)$。

即，$h_{\Theta}(x) \approx x_1 \text{AND} x_2$。

![图3](/images/Machine_Learning_Lecture4_3.png)

同理，通过对$OR$训练集的学习，可以获得$\Theta = \begin{bmatrix} -10 \\\\\\ 20 \\\\\\ 20 \end{bmatrix}$。

即，$h_{\Theta}(x) = g(-10 + 20 x_1 + 20 x_2)$。

即，$h_{\Theta}(x) \approx x_1 \text{OR} x_2$。

![图4](/images/Machine_Learning_Lecture4_4.png)

同样的，下图的神经元可以被视为等同于逻辑非($NOT$)。

![图5](/images/Machine_Learning_Lecture4_5.png)

现在，可以利用这些神经元来组合成为复杂的神经网络以实现更复杂的运算——同或运算(`XNOR`)。

同或运算公式为：$x_1 \text{XNOR} x_2 = (x_1 \text{AND} x_2) \text{OR}((\text{NOT}x_1) \text{AND} (\text{NOT}x_2))$。

如下图，先构造能表达$x_1 \text{AND} x_2$和$(\text{NOT}x_1) \text{AND} (\text{NOT}x_2)$的神经元，再这两个神经元与表示$x_1 \text{OR} x_2$的神经元进行组合：

![图6](/images/Machine_Learning_Lecture4_6.png)

这样就得到了一个能实现$XNOR$运算功能的神经网络。

按照这种方法可以逐渐构造出越来越复杂的函数，也能得到更加厉害的特征值。

这就是神经网络的厉害之处。

## 多元分类

当有不止两种分类时（也就是$y=1,2,3….$），比如要训练一个神经网络算法来识别路人、汽车、摩托车和卡车，在输出层应该有4个值。例如，第一个值为1或0用于预测是否是行人，第二个值用于判断是否为汽车。

输入向量$x$有三个维度，两个中间层，输出层4个神经元分别用来表示4类，也就是每一个数据在输出层都会出现$\begin{bmatrix} a & b & c & d \end{bmatrix}^T$，且$a,b,c,d$中仅有一个为1，表示当前类。下面是该神经网络的结构示例：

![图7](/images/Machine_Learning_Lecture4_7.png)