---
layout: post
title: paperwithcode网站关于Hand Gesture Recognition的模型和数据集整理
description: 手势识别的文章和数据集的整理
tags:
  - paper
catagories:
  - Deep Learning
mathjax: true
abbrlink: f209dfc7
date: 2021-05-06 10:40:13
---

# Papers

All information comes from this [website](https://paperswithcode.com/task/hand-gesture-recognition).



## PointLSTM

#### Paper

[An Efficient PointLSTM for Point Clouds Based Gesture Recognition](https://openaccess.thecvf.com/content_CVPR_2020/papers/Min_An_Efficient_PointLSTM_for_Point_Clouds_Based_Gesture_Recognition_CVPR_2020_paper.pdf)

#### Code

[Blueprintf](https://github.com/Blueprintf/pointlstm-gesture-recognition-pytorch)

#### Dataset

* [NVGesture](#NVGesture)
* [SHREC-2017](#SHREC-2017)

## 8-MFFs-3f1c

#### Paper

[Motion Fused Frames: Data Level Fusion Strategy for Hand Gesture Recognition](https://arxiv.org/pdf/1804.07187v2.pdf)


#### Code

[okankop](https://github.com/okankop/MFF-pytorch)

#### Dataset

* [Jester](#Jester)

* [Chalearn LAP IsoGD](#Chalearn LAP IsoGD)
* [NVGesture](#NVGesture)

## ResNeXt-101

#### Paper

[Real-time Hand Gesture Detection and Classification Using Convolutional Neural Networks](https://arxiv.org/pdf/1901.10323v3.pdf)

#### Code

[ahmetgunduz](https://github.com/ahmetgunduz/Real-time-GesRec)

#### Dataset

* [EgoGesture](#EgoGesture)
* [NVGesture](#NVGesture)
* [Jester](#Jester)

## MTUT

#### Paper

[Improving the Performance of Unimodal Dynamic Hand-Gesture Recognition with Multimodal Training](https://arxiv.org/pdf/1812.06145v2.pdf)


#### Code

[sbhonde1](https://github.com/sbhonde1/Multimodal-Learning)

#### Dataset

* [VIVA Hand Gesture Datasets](#VIVA Hand Gesture Datasets)
* [EgoGesture](#EgoGesture)
* [NVGesture](#NVGesture)

## Key Frames + Feature Fusion

#### Paper

[Fast and Robust Dynamic Hand Gesture Recognition via Key Frames Extraction and Feature Fusion](https://arxiv.org/pdf/1901.04622v1.pdf)

#### Code

[Ha0Tang](https://github.com/Ha0Tang/HandGestureRecognition)

#### Dataset

* [Cambridge Hand Gesture](#Cambridge Hand Gesture)
* [Northwestern Hand Gesture](#Northwestern Hand Gesture)

## DG-STA

#### Paper

[Construct Dynamic Graphs for Hand Gesture Recognition via Spatial-Temporal Attention](https://arxiv.org/pdf/1907.08871v1.pdf)

#### Code

[yuxiaochen1103](https://github.com/yuxiaochen1103/DG-STA)

#### Dataset

* [SHREC-2017](#SHREC-2017)

* [Dynamic Hand Gesture 14-28 dataset](#Dynamic Hand Gesture 14-28 dataset)

## F-BLSTM/F-BGRU

#### Paper

[Deep Fisher Discriminant Learning for Mobile Hand Gesture Recognition](https://arxiv.org/pdf/1707.03692v1.pdf)

#### Code

[chriswegmann](https://github.com/chriswegmann/drone_steering)

#### Dataset

* MGD

***

***





 

# Datasets



## [NVGesture](https://docs.google.com/forms/d/e/1FAIpQLSc7ZcohjasKVwKszhISAH7DHWi8ElounQd1oZwORkSFzrdKbg/viewform)

#### Paper

> [Gupta P, Kautz K. Online detection and classification of dynamic hand gestures with recurrent 3d convolutional neural networks[C]//CVPR. 2016, 1(2): 3.](https://openaccess.thecvf.com/content_cvpr_2016/papers/Molchanov_Online_Detection_and_CVPR_2016_paper.pdf)

### Model

[PointLSTM](#PointLSTM)，[8-MFFs-3f1c](#8-MFFs-3f1c)，[ResNeXt-101](#ResNeXt-101)，[MTUT](#MTUT)

包含25种手势【include moving either the hand or two fingers up, down, left or right[8]; clicking with the index finger[1]; beckoning[1]; opening or shaking the hand[2]; showing the index finger, or two or three fingers[3]; pushing the hand up, down, out or in[4]; rotating two fingers clockwise or counterclockwise[2]; pushing two fingers forward[1]; closing the hand twice[1]; and showing “thumb up” or “OK”[2].】，每种手势类型均用于人机界面并由多个传感器和视点记录。

 在室内由明亮和昏暗的人工照明的汽车模拟器种捕获了连续的数据流，总共包含1532个动态手势。

20位受试者参与数据收集，有些受试者进行了两次记录，有些则进行了部分训练。

使用SoftKinetic DS325传感器获取前视彩色和深度视频，并使用顶部安装的DUO 3D传感器记录一对立体声IR流。

另外，我们从色流计算出密集的光流，并从红外立体对计算出红外视差图。

我们按主题将数据随机分为训练（70％）和测试（30％）集，从而产生1050个训练和482个测试视频。

## [SHREC-2017](http://www-rech.telecom-lille.fr/shrec2017-hand/)

#### Paper

> [SHREC 2017: 3D Hand Gesture Recognition Using a Depth and Skeletal Dataset](https://diglib.eg.org/bitstream/handle/10.2312/3dor20171049/033-038.pdf?sequence=1&isAllowed=y)

#### Model

[PointLSTM](#PointLSTM)，[DG-STA](#DG-STA)

两种方式[一根手指+全部手]演示14种手势，每个手势由28个志愿者用两种方式演示1-10次。序列标签：手势、手指数量、演示者和实验。序列每帧包含：深度图像、2D深度图像空间和3D世界空间中的22个关节的坐标都形成了完整的手型骨骼。



因特尔实感短距离深度摄像头用于收集数据集。



深度图像和手部骨骼以30fps的速度捕获，分辨率为640*480。样本手势的长度范围为20-50f



##  [Jester](https://20bn.com/datasets/something-something)

#### Model

[8-MFFs-3f1c](#8-MFFs-3f1c)，[ResNeXt-101](#ResNeXt-101)

#### Paper

>[ON THE EFFECTIVENESS OF TASK GRANULARITY FOR TRANSFER LEARNING](https://arxiv.org/pdf/1804.09235.pdf)
>
>[The “something something” video database for learning and evaluating visual common sense](https://arxiv.org/pdf/1706.04261.pdf)

20BN-SOMETHING-SOMETHING数据集是一个带有标签的视频剪辑的大集合，这些视频剪辑向**人们**展示了**人类对日常对象执行预定义的基本动作**。该数据集是由大量的人群工作者创建的。



| Total number of videos | 220,847 |
| ---------------------- | ------- |
| Training Set           | 168,913 |
| Validation Set         | 24,777  |
| Test Set (w/o labels)  | 27,157  |
| Labels                 | 174     |

## [Chalearn LAP IsoGD](http://www.cbsr.ia.ac.cn/users/jwan/database/isogd.html)

#### Model

[8-MFFs-3f1c](#8-MFFs-3f1c)

#### Paper

> [ChaLearn Looking at People RGB-D Isolated and Continuous Datasets for Gesture Recognition](http://www.cbsr.ia.ac.cn/users/jwan/papers/CVPRW2016_JunWan.pdf)

47933个RGB-D手势视频（约9G）。每个RGB-D视频仅代表一个手势，由21个不同的人执行的249个手势标签。

| Sets       | # of Labels | # of Gestures | # of RGB Vidoes | # of Depth Vidoes | # of Performers | Label Provided |
| ---------- | ----------- | ------------- | --------------- | ----------------- | --------------- | -------------- |
| Training   | 249         | 35878         | 35878           | 35878             | 17              | Yes            |
| Validation | 249         | 5784          | 5784            | 5784              | 2               | No             |
| Testing    | 249         | 6271          | 6271            | 6271              | 2               | No             |

## [EgoGesture](http://www.nlpr.ia.ac.cn/iva/yfzhang/datasets/egogesture.html)

#### Model

[ResNeXt-101](#ResNeXt-101)，[MTUT](#MTUT)

#### Paper

> [EgoGesture: A New Dataset and Benchmark for Egocentric Hand Gesture Recognition]()

EgoGesture是用于以自我为中心的手势识别的多模式大规模数据集。该数据集不仅为分割数据中的手势分类提供了测试平台，而且还为连续数据中的手势检测提供了测试平台。



数据集包含来自50个不同主题的2,081个RGB-D视频，24,161个手势样本和2,953,224帧。我们设计了83种静态或动态手势，重点是与可穿戴设备的交互。这些视频是从6种不同的室内和室外场景中收集的。我们还考虑了人们在行走时执行手势的情况。



我们选择英特尔实感SR300作为我们的以自我为中心的摄像头，因为它体积小并且集成了RGB和深度模块。两种模式的视频以640ⅹ480像素的分辨率和30 fps录制。要求戴着RealSense摄像头并带上皮带的受试者在会话中连续执行9-14个手势，并记录为视频。由于执行手势的顺序是随机生成的，因此视频可用于评估连续流中的手势检测。除了类别标签的注释外，还手动标记了每个手势样本的开始和结束帧索引，这为分段的手势分类提供了测试平台。

![Figure 2](http://www.nlpr.ia.ac.cn/iva/yfzhang/datasets/ego_gesture/Gestures_all.png)

## [VIVA Hand Gesture Datasets](https://lttm.dei.unipd.it/downloads/gesture/)

#### Model 

[MTUT](#MTUT)

### Microsoft Kinect and Leap Motion

通过Leap Motion和Kinect两种传感器设备获取多种不同手势。可用于构建和评估混合手势识别系统。



该数据集包含由14个不同的人执行的手势，每个人执行10个不同的手势，每个手势重复10次，总共1400个手势。



![image-20210507103615308](images/image-20210507103615308.png)

### Creative Senz3D

通过Creative Senz3D相机获取几种不同的静态手势。可以用于测试使用HandPoseGenerator生成的合成数据训练的多分类SVM手势分类器的预测准确性。



数据集包含由4个不同的人执行的手势，每个人执行11个不同的手势，每个手势重复30次，总共有1320个样本。对于每个样本，颜色，深度和置信度框均可用。

![image-20210507104057303](images/image-20210507104057303.png)



## [Cambridge Hand Gesture](https://labicvl.github.io/ges_db.htm)

#### Model

[Key Frames + Feature Fusion ](#Key Frames + Feature Fusion)

#### Paper

> [Tensor Canonical Correlation Analysis for Action Classification](https://labicvl.github.io/docs/pubs/TK_CVPR_2007.pdf)  2007年的上古文章了

数据集由900个图像序列组成，这些图像序列包含9种手势类别，由3种原始手形和3种原始运动定义。因此，此数据集的目标任务是一次对不同的形状以及不同的运动进行分类。

![img](https://labicvl.github.io/images/work/ges_class2.jpg)



每个类别包含100个图像序列（5个不同的照明x 10个任意运动x 2个对象）。每个序列都记录在固定照相机的前面，该照相机在空间和时间上具有大致隔离的手势。因此，空间和时间对齐中的类内相当大的变化会反映到数据集。有9类的典型样本序列，5种不同的照明原型。

![img](https://labicvl.github.io/images/work/ges_db2.jpg)

![img](https://labicvl.github.io/images/work/ges_db3.jpg)

## [Northwestern Hand Gesture]()

#### Model

[Key Frames + Feature Fusion ](#Key Frames + Feature Fusion)

#### Paper

> [Motion Divergence Fields for Dynamic Hand Gesture Recognition ](http://users.ece.northwestern.edu/~ganghua/publication/FG11.pdf) 2011



## [Dynamic Hand Gesture 14-28 dataset](http://www-rech.telecom-lille.fr/DHGdataset/)

#### Model

[DG-STA](#DG-STA)

#### Paper

> [Dynamic Hand Gesture Recognition using Skeleton-based Features](http://www-rech.telecom-lille.fr/DHGdataset/...)

以两种方式执行的14个手势序列：使用一根手指和整个手。每个手势由20位参与者按上述两种方式执行5次，结果为2800个序列。所有参与者都是右撇子。序列按照其手势，使用的手指数，演奏者和试验进行标记。



序列的每一帧都包含一个深度图像，在2D深度图像空间和3D世界空间中的22个关节的坐标均形成完整的手形骨骼。英特尔实感短距离深度摄像头(The Intel RealSense short range depth camera)用于收集我们的数据集。



深度图像和手部骨骼以每秒30帧的速度捕获，分辨率为640x480。样例手势的长度范围为20到50帧。


