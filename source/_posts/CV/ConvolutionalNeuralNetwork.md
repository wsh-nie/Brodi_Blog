---
layout: post
title: "卷积神经网络"
date: 2020-09-14 19:36:26
draft: false
description: "卷积、图像去噪、边缘提取"
tags: 
- CV
- AI
categories: 
- CV
mathjax: true
---

## 卷积与图像去噪

### 图像去噪与卷积

噪声：一个点与周围点差别比较大

去噪简单思想：周围的点加权平均一下

卷积核：即权值集合

卷积定义：令$F$为图像，$H$为卷积核，$F$与$H$的卷积记为$R=F * H$。

$$
R_{ij} = \sum_{u,v} H_{i-u,j-v} F_{u,v}
$$

使用$H$将$F$卷积到域$R$

先将卷积核进行$108°$翻转，再去图像上滤波。如果不翻转则是进行相关操作。

卷积性质：

叠加性：$filter(f_1 + f_2) = filter(f_1) + filter(f_2)$

平移不变性：$filter( shift(f) ) = shift( filter(f) )$

交换律、结合律、分配律和标量乘。

边界填充：图像边界的卷积需要进行填充。

卷积操作后的图像要小于输入时图像，通过边界填充，我们可以实现卷积前后图像的尺寸不变。最常用的边界填充就是常数填充。

原图 - 平滑模板 = 边缘图
原图 + 边缘图 = 锐化操作

### 高斯卷积核

平均卷积核卷积后的图像产生了一些水平和竖直方向的条状**振铃**。

解决振铃的方法：根据领域像素与中心得远近程度分配权重，离中心近的权重大。

高斯核：

$$
G_{\sigma} = \frac{1}{2\pi \sigma^2} e^{ - \frac{(x^2 + y^2)}{2 \sigma^2}}
$$

生成步骤：
1. 确定卷积核的尺寸，比如$5 * 5$；
2. 设置高斯函数的标准差，比如$\sigma = 1$；
3. 计算卷积核各个位置权重值；
4. 对权重值进行归一化。

卷积核参数总结：

大方差或者大尺寸卷积核平滑能力强；  
经验法则：将卷积核的半窗宽度设置为$3\sigma$，最终卷积模板尺寸为$2 * 3 \sigma + 1$。$3\sigma$以外的值接近于$0$，无意义。

高斯卷积核的特性

去除图像中的"高频"成分(低通滤波器)

两个高斯卷积核卷积后得到的还是高斯卷积核  
使用多次小方差卷积核连续卷积，可以得到与大方差卷积核相同的结果  
使用标准差为$\sigma$的高斯核进行两次卷积与使用标准差$\sigma \sqrt 2$的高斯核进行一次卷积相同。

可分离  
可分解为两个一维高斯的乘积，可极大的减少计算量。

卷积操作运算量

用尺寸为$m * m$的卷积核卷积一个尺寸为$n * n$的图像，其计算复杂度是多少？

直接计算是$O(m^2n^2)$，改成两个一维高斯核卷积，复杂度为$O(mn^2)$

### 图像噪声与中值滤波器

#### 噪声

椒盐噪声：黑色像素和闹色像素随机出现。

脉冲噪声：白色像素随机出现。

高斯噪声：噪声强度变化服从高斯分布。

$$
\begin{align}
&高斯噪声： \hat f (x, y) = f(x, y) + \eta (x, y) \\\\\\
& \eta (x, y) \text{~} N(\mu,\sigma), 通常\mu = 0, \sigma很小
\end{align}
$$

对于椒盐噪声和脉冲噪声，高斯核卷积无法完全去噪。需要使用中值滤波器。

中值滤波器使用的卷积核无权重，对范围内的数值进行排序，选择权值中间的值替换。

![中值滤波器](/images/CV/ConvolutionalNeuralNetwork_1.png)

高斯卷积核是线性操作，中值滤波器不是线性操作。

## 卷积与边缘提取

### 边缘

边缘：图像中亮度明显而急剧变化的点

为什么要研究边缘？  
编码图像中的语义与形状信息  
相对于图像表示，边缘表示显然更加紧凑

边缘的种类：

![边缘的种类](/images/CV/ConvolutionalNeuralNetwork_2.png)

### 边缘检测

![边缘的检测](/images/CV/ConvolutionalNeuralNetwork_3.png)

检测边缘需要用到图像的导数。

图像求导公式：$\epsilon = 1$

$$
\frac{\partial f(x, y)}{\partial x} \approx \frac{f(x + 1, y) - f(x, y)}{1}
$$

使用卷积核则可表示为：

$$
\frac{\partial f(x, y)}{\partial x} \Rightarrow \begin{bmatrix} -1 & 1 \end{bmatrix}
$$
$$
\frac{\partial f(x, y)}{\partial y} \Rightarrow \begin{bmatrix} -1 \\\\\\ 1 \end{bmatrix} or \begin{bmatrix} 1 \\\\\\ -1 \end{bmatrix}
$$

### 图像的梯度

梯度指向灰度变换最快的方向，即两个方向的导数向量。

$$
\nabla f = \begin{bmatrix} \frac{\partial f}{\partial x}& \frac{\partial f}{\partial y} \end{bmatrix}  
$$

梯度的模：

$$
|| \nabla f || = \sqrt{(\frac{\partial f}{\partial x})^2+(\frac{\partial f}{\partial y})^2}
$$

梯度的模越大，是边缘的概率越大

![边缘检测样例](/images/CV/ConvolutionalNeuralNetwork_4.png)

### 噪声的影响

噪声导致图像的导数极大值过多，不能表示边缘。

故需要先去噪使得图像平滑，再对去噪后的信号求导。

对于一个具有高斯噪声的图像，由于降噪和微分都是卷积，而卷积具备结合性，故：

$$
\frac{d}{d x}(f * g) = f * \frac{d}{dx}g
$$

![高斯一阶偏导卷积核](/images/CV/ConvolutionalNeuralNetwork_5.png)

方差小，边缘更细腻。

### 高斯核 VS. 高斯一阶偏导核

高斯核：

消除高频成分(低通滤波器)  
卷积核中的权值不可为复数  
权值总和为1(恒定区域不受卷积影响)

高斯一阶偏导核：

高斯的导数  
卷积核中的权值可以是负数  
权值总和是0(恒定区域无响应)  
高对比度点的响应值大

### 非最大值抑制

高斯操作边缘提取遇到平滑变化的图像，边缘不明显。

对于图像的非0点p，沿着**梯度方向**，如果p点的梯度强度比前后点的梯度强度大，则保留；否则删除。

![非最大值抑制](/images/CV/ConvolutionalNeuralNetwork_6.png)

q,r点可能不在像素点，则用周围点加权求和。

### Canny边缘检测器

![梯度门限问题](/images/CV/ConvolutionalNeuralNetwork_7.png)

采用双阈值，先用高阈值，然后再用低阈值，当低阈值的边链接两个高阈值的边，则保留。