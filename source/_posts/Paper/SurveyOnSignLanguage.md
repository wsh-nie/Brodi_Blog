---
layout: post
title: 手势识别相关文献（一）
description: 关于手语识别的一些文章整理
abbrlink: 635dd607
date: 2021-05-07 16:55:17
tags:
  - paper
catagories:
  - Deep Learning
mathjax: true
---


## INCLUDE: A Large Scale Dataset for Indian Sign Language Recognition

[文章链接](https://wvpn.ustc.edu.cn/https/77726476706e69737468656265737421f4fb0f9d243d265f6c0f/doi/pdf/10.1145/3394171.3413528)

**Information**

作者：Advaith Sridhar, Rohith Gandhi Ganesan, Pratyush Kumar, Mitesh Khapra
机构：IIT Madras Chennai 印度理工学院
期刊：In Proceedings of the 28th ACM International Conference on Multimedia (MM ’20)

**Abstract**

>介绍了印度词汇手语数据集-INCLUDE-一个ISL数据集，其中包含来自27个单词符号，来自15个不同单词类别的4,287个视频中的27万帧。在经验丰富的标注数据者的帮助下记录INCLUDE，以使其与自然条件极为相似。从单词类别中选择50个单词符号的子集，以定义INCLUDE-50，以便通过超参数调整快速评估SLR方法。
>
>评估了几种深度神经网络，它们结合了用于增强，特征提取，编码和解码的不同方法。性能最佳的模型在INCLUDE-50数据集上的准确度达到94.5％，在INCLUDE数据集上的准确度达到85.6％。这个最好的模型使用预训练的特征提取器和编码器，仅训练解码器。


## Word-level Deep Sign Language Recognition from Video: A New Large-scale Dataset and Methods Comparison

[文章链接](https://openaccess.thecvf.com/content_WACV_2020/papers/Li_Word-level_Deep_Sign_Language_Recognition_from_Video_A_New_Large-scale_WACV_2020_paper.pdf)

[Code](https://github.com/verashira/TSPNet)

**Information**

作者：[Dongxu Li](https://dxli94.github.io/) , Cristian Rodriguez Opazo, Xin Yu, Hongdong Li
机构：The Australian National University, Australian Centre for Robotic Vision (ACRV)
期刊：WACV 2020

**Abstract**

>介绍了一个新的大规模单词级美国手语（WLASL）视频数据集，其中包含由100多个签名者执行的2000多个单词。该数据集将公开提供给研究社区。
>
>基于这个新的大规模数据集，实现并比较了两种不同的模型，即（i）基于整体视觉外观的方法和（ii）基于2D人体姿势的方法。此外，我们还提出了一种新颖的基于姿势的时间图卷积网络（Pose-TGCN），该模型同时对人类姿势轨迹中的空间和时间依赖性进行建模，从而进一步提高了基于姿势的方法的性能。
>
>我们的结果表明，基于姿势的模型和基于外观的模型在2,000个单词/光泽度的前10位准确性上可实现高达62.63％的可比性能，这证明了我们数据集的有效性和挑战性。


## Recognizing Camera Wearer from Hand Gestures in Egocentric Videos

[文章链接](https://www.cse.iitd.ac.in/~chetan/papers/daksh-mm-2020.pdf)
[Demo](https://egocentricbiometric.github.io/)


**Information**

作者：Daksh Thapar, Aditya Nigam, Chetan Arora
机构：Indian Institute of Technology Mandi
期刊：In Proceedings of the 28th ACM International Conference on Multimedia (MM ’20)

**Abstract**

>已经证明，以自我为中心的摄像机可以间接捕获佩戴者的步态，这些步态可用于基于他们的以自我为中心的视频来识别佩戴者。在这项工作中，我们的研究表明，即使是佩戴者的手势（通过以自我为中心的视频看到）也泄漏了佩戴者的身份。
>
>我们设计了一个模型，以从以自我为中心的视频中提取和匹配手势签名。我们在EPIC厨房数据集中展示了威胁。
>
>1.根据相同的活动，可以识别出佩戴者的准确度高达73％；2. 手势签名会跨活动转移，即，即使我们的模型在训练时未看到佩戴者的“切割”活动，但看到了其他活动（例如“洗”，“混合”等），模型仍可以识别；3. 手势功能甚至可以跨对象转移，即，即使模型没有看到某个对象的任何活动，人们仍然可以验证佩戴者（开放设置），并预测同一佩戴者已经执行了两项活动。平均错误率为15.21％

## Quantitative Survey of the State of the Art in Sign Language Recognition

[文章链接](https://arxiv.org/pdf/2008.09918v2.pdf)

**Information**

作者：Oscar Koller
机构：Microsoft
期刊：Arxiv 2020

**Abstract**

>这项工作提出了一项荟萃研究，涉及约300篇已发表的手语识别论文，并有400多个实验结果。

