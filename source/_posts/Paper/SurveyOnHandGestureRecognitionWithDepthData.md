---
layout: post
title: 手势识别相关文献（三）
description: 使用深度信息的手势识别文章
abbrlink: be986943
date: 2021-05-08 11:49:02
tags:
  - paper
catagories:
  - Deep Learning
mathjax: true
---

## Enhanced Computer Vision with Microsoft Kinect Sensor: A Review

[文本链接](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.672.2534&rep=rep1&type=pdf)


**Information**

作者：Jungong Han, Ling Shao, Dong Xu, Jamie Shotton
机构：
期刊： IEEE Transactions on Cybernetics(2013)

**Abstract**

>本文对最近基于Kinect的计算机视觉算法和应用程序进行了全面综述。根据可以通过Kinect传感器解决或增强的视觉问题的类型，对已审查的方法进行了分类。涵盖的主题包括预处理，对象跟踪和识别，人类活动分析，手势分析以及室内3-D映射。对于每种方法，我们概述了它们的主要算法贡献，并总结了它们与RGB方法相比的优点/不同之处。最后，我们概述了该领域的挑战和未来的研究趋势。本文有望作为基于Kinect的计算机视觉研究人员的教程和参考资料来源

## Combining multiple depth-based descriptors for hand gesture recognition

[文本链接](https://lttm.dei.unipd.it/nuovo/Papers/14_PRL_hand.pdf)

**Information**

作者：Fabio Dominio, Mauro Donadeo, Pietro Zanuttigh
机构：Dapartment of Information Engineering, University of Padova, Italy
期刊：Pattern Recognition Letters(2013)

**Abstract**

> 当前的低成本实时深度相机获取的深度数据提供了有关手势的更多信息，可用于手势识别目的。根据这一原理，本文介绍了一种基于深度信息的新颖手势识别方案。首先从获取的数据中提取手，并将其分为手掌和手指区域。然后提取四组不同的特征描述符，以说明不同的线索，例如指尖距手中心和手掌平面的距离，手轮廓的曲率和手掌区域的几何形状。最终，采用多类SVM分类器来识别执行的手势。

## No frame left behind: Full Video Action Recognition

[文章链接](https://arxiv.org/pdf/2103.15395.pdf)

**Information**

作者：Xin Liu, Silvia L. Pintea, Fatemeh Karimi Nejadasl, Olaf Booij, Jan C. van Gemert
机构：Computer Vision Lab, Delft University of Technology【荷兰 | 代尔夫特理工大學】
期刊： CVPR 2021


**Abstract**

>并不是所有的视频帧都能提供同样的信息来识别一个动作。当动作发展到数百帧以上时，在所有视频帧上训练深度网络在计算上是不可行的。一种常见的启发式方法是统一抽取少量的视频帧，并使用这些帧来识别动作。相反，这里我们提出了完整的视频动作识别，并考虑所有的视频帧。为了使这一计算易于处理，我们首先根据与分类任务相关的相似度，沿着时间维度聚类所有的框架激活，然后在时间上将聚类中的框架聚合为较少数量的表示。我们的方法是端到端可训练和计算效率，因为它依赖于时间局部聚类结合快速汉明距离在特征空间。我们对UCF101、HMDB51、Breakfast和Something-Something  V1和V2进行了评估，并与现有的启发式帧抽样方法进行了比较。