---
layout: post
title: 手势识别相关文献（二）
description: 不使用深度信息的手势识别文章
abbrlink: 13b9270c
date: 2021-05-08 11:40:02
tags:
  - paper
catagories:
  - Deep Learning
mathjax: true
---


## 3D Hand Shape and Pose Estimation from a Single RGB Image 【estimation】

[文章链接](https://wvpn.ustc.edu.cn/https/77726476706e69737468656265737421f9f244993f20645f6c0dc7a59d50267b1ab4a9/stamp/stamp.jsp?tp=&arnumber=8953612)

**Information**

作者：Liuhao Ge, Zhou Ren, Yuncheng Li ... Junsong Yuan
机构：Nanyang Technological University, Wormpex AI Research, Snap Inc, State University of New York at Buffalo
期刊：CVPR 2019

**Abstract**

>从单个RGB图像估计完整的3D手形和姿势。们提出了一种基于图卷积神经网络（Graph CNN）的方法来重建手表面的完整3D网格，该网格包含更丰富的3D手形状和姿势信息。为了在完全监督的情况下训练网络，我们创建了一个包含地面真实3D网格和3D姿势的大规模综合数据集。当在没有3D地面真实性的情况下对真实数据集上的网络进行微调时，我们通过利用深度图作为训练中的弱监督，提出了一种弱监督方法。

## 3D Hand Shape and Pose from Images in the Wild 【Estimation】

[文章链接](https://wvpn.ustc.edu.cn/https/77726476706e69737468656265737421f9f244993f20645f6c0dc7a59d50267b1ab4a9/stamp/stamp.jsp?tp=&arnumber=8953961&tag=1)

**Information**

作者：Adnane Boukhayma, Rodrigo de Bem, Philip H.S. Torr
机构：University of Oxford, Federal University of Rio Grande
期刊：CVPR 2019

**Abstract**

>我们在这项工作中提出了第一个基于端到端深度学习的方法，该方法可以从野外的RGB图像中预测3D手的形状和姿势。

## Dynamic Gesture Recognition with Pose-based CNN Features derived from videos using LSTM

[文章链接](https://wvpn.ustc.edu.cn/https/77726476706e69737468656265737421f4fb0f9d243d265f6c0f/doi/pdf/10.1145/3293353.3293398)

**Information**

作者： Kankana Ray, Rajiv R. Sahay
机构： Computational Vision Laboratory, Indian Institute of Technology Kharagpur
期刊： ICVGIP 2018 [期刊似乎不咋地]


**Abstract**

>在这项工作中，我们解决了使用基于姿势的视频描述符进行动态手势识别的问题。所提出的方法将视频帧作为输入并提取特定姿势图像区域，这些区域将由预训练的卷积神经网络（CNN）进一步处理，以得出每个帧的基于姿势的描述符。通过学习特征之间的长期时空关系，从头开始训练长期短期记忆（LSTM）网络以进行动态手势分类。我们还证明，只有使用视频数据（RGB帧和光流），才能设计出一种有效的模型来识别动态手势。

## Convolutional Two-Stream Network Fusion for Video Action Recognition

[文章链接](https://openaccess.thecvf.com/content_cvpr_2016/papers/Feichtenhofer_Convolutional_Two-Stream_Network_CVPR_2016_paper.pdf)

**Information**

作者：CHristoph Feichtenhofer, Axel Pinz, Andrew Zisserman
机构：Graz University of Technology
期刊：CVPR 2016

**Abstract**

>卷积神经网络（ConvNets）在视频中进行人类动作识别的最新应用提出了用于合并外观和运动信息的不同解决方案。我们研究了在空间和时间上融合ConvNet塔的多种方法，以便最好地利用此时空信息。我们得出以下发现：（i）可以在卷积层融合时空网络，而不是在softmax层进行融合，而不会损失性能，但可以节省大量参数；（ii）最好在最后一个卷积层上在空间上融合此类网络，而不是在以前的卷积层上融合，并且在类预测层上额外融合可以提高准确性；最后（iii）在时空邻域上合并抽象卷积特征进一步提高了性能。基于这些研究，我们提出了一种新的ConvNet架构，用于视频片段的时空融合，并在标准基准上评估其性能，该架构可实现最新的结果。

## Two-Stream Convolutional Networks for Action Recognition in Video

[文章链接](https://arxiv.org/pdf/1406.2199.pdf)

**Information**

作者：Karen Simonyan, Andrew Zisserman
机构：Visual Genmetry Group, University of Oxford
期刊：

**Abstract**

>首先，我们提出了一个包含空间和时间网络的两流ConvNet体系结构。其次，我们证明了在有限的训练数据的情况下，经过多帧密集光流训练的ConvNet仍可以实现非常好的性能。最后，我们表明将多任务学习应用于两个不同的动作分类数据集，可用于增加训练数据量并提高两者的性能。