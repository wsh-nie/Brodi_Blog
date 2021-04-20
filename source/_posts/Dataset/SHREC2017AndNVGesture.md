---
layout: post
title: SHREC2017和NVGesture数据集
description: 整理数据集的来源和处理方法
abbrlink: aa7ee9a8
date: 2021-04-20 13:38:13
tags:
  - dataset
categories:
  - Deep Learning
mathjax: true
---

# SHREC 2017

## Dataset Info

> The dataset contains sequences of **14 hand gestures** performed in two ways: using **one finger** and the **whole hand**. Each gesture is performed between 1 and 10 times by 28 participants in 2 ways - described above - , resulting in 2800 sequences. All participants are right handed. **Sequences are labelled following their gesture, the number of fingers used, the performer and the trial.** ***Each frame of sequences contains a depth image, the coordinates of 22 joints both in the 2D depth image space and in the 3D world space forming a full hand skeleton.*** The Intel RealSense short range depth camera is used to collect our dataset. The depth images and hand skeletons were captured at 30 frames per second, with a resolution of the depth image of 640x480. The length of sample gestures ranges from 20 to 50 frames.

* 两种方式[一根手指+全部手]演示14种手势
* 每个手势由28个志愿者用两种方式演示1-10次
* 序列标签：手势、手指数量、演示者和实验
* 序列每帧包含：深度图像、2D深度图像空间和3D世界空间中的22个关节的坐标都形成了完整的手型骨骼
* 因特尔实感短距离深度摄像头用于收集数据集
* 深度图像和手部骨骼以30fps的速度捕获，分辨率为640*480
* 样本手势的长度范围为20-50f

## File Architecture

```
|-- SHREC2017
|    |-- gesture_1
|        |-- finger_1
|            |-- subject_1
|                |-- essai_1
|                   |-- 0_depth.png
|                   ...
|                   |-- 94_depth.png
|                   |-- general_informations.txt
|                   |-- skeletons_images.txt
|                   |-- skeletons_world.txt
|                |-- essai_2
|                |-- essai_3
|                |-- essai_4
|                |-- essai_5
|            |-- subject_2
|            ...
|            |-- subject_28
|        |-- finger_2
|    |-- gesture_2
...
|    |-- gesture_28
```

# NVIDIA Dynamic Hand Gesture Dataset
> Given the limitations of existing datasets, to validate our proposed gesture recognition algorithm, we acquired a large dataset of 25 gesture types, each intended for humancomputer interfaces and **recorded by multiple sensors and viewpoints.** We captured continuous data streams, **containing a total of1532 dynamic hand gestures**, indoors in a car simulator with both bright and dim artificial lighting. A total of 20 subjects participated in data collection, some with two recorded sessions and some with partial sessions. Subjects performed gestures with their right hand while observing the simulator’s display and controlling the steering wheel with their left hand. An interface on the display prompted subjects to perform each gesture with an audio description and a 5s sample video of the gesture. Gestures were prompted in random order with each type requested 3 times over the course of a full session. 
> 
> **Gestures include moving either the hand or two fingers up, down, left or right; clicking with the index finger; beckoning; opening or shaking the hand; showing the index finger, or two or three fingers; pushing the hand up, down, out or in; rotating two fingers clockwise or counterclockwise; pushing two fingers forward; closing the hand twice; and showing “thumb up” or “OK”.**
>
> **We used the SoftKinetic DS325 sensor to acquire frontview color and depth videos and a top-mounted DUO 3D sensor to record a pair of stereo-IR streams.** In addition, we computed dense optical flow from the color stream and the IR disparity map from the IR-stereo pair. We randomly split the data by subject into training (70%) and test (30%) sets, resulting in 1050 training and 482 test videos.

* 包含25种手势【include moving either the hand or two fingers up, down, left or right[8]; clicking with the index finger[1]; beckoning[1]; opening or shaking the hand[2]; showing the index finger, or two or three fingers[3]; pushing the hand up, down, out or in[4]; rotating two fingers clockwise or counterclockwise[2]; pushing two fingers forward[1]; closing the hand twice[1]; and showing “thumb up” or “OK”[2].】，每种手势类型军用于人机界面并由多个传感器和视点记录
* 在室内由明亮和昏暗的人工照明的汽车模拟器种捕获了连续的数据流，总共包含1532个动态手势
* 20位受试者参与数据收集，有些受试者进行了两次记录，有些则进行了部分训练
* 使用SoftKinetic DS325传感器获取前视彩色和深度视频，并使用顶部安装的DUO 3D传感器记录一对立体声IR流
* 另外，我们从色流计算出密集的光流，并从红外立体对计算出红外视差图
* 我们按主题将数据随机分为训练（70％）和测试（30％）集，从而产生1050个训练和482个测试视频

## File Architecture

```
|-- Nvidia
|    |-- Video_data
|        |-- class_01
|            |-- subject1_r0
|                |-- duo_left_log.txt
|                |-- duo_left.avi
|                |-- duo_right_log.txt
|                |-- duo_right.avi
|                |-- nucleus.txt
|                |-- sk_color_log.txt
|                |-- sk_color.avi
|                |-- sk_depth_log.txt
|                |-- sk_depth.avi
|                |-- sk_wrapped_log.txt
|                |-- sk_wrapped.avi
|            |-- subject1_r1
|            ...
|            |-- subject1_r5
|            |-- subject2_r0
|            ...
|            |-- subject2_r2
|            |...
|            |-- subject20_r5
|        |-- class_02
|        ...
|        |-- class_25
|    |-- nvgesture_test_correct_cvpr2016.v2.lst
|    |-- nvgesture_test_correct_cvpr2016.lst
|    |-- nvgesture_train_correct_cvpr2016.v2.lst
|    |-- nvgesture_train_correct_cvpr2016.lst
```



# 参考

1. [SHREC 2017: 3D Hand Gesture Recognition Using a Depth and Skeletal Dataset](http://www-rech.telecom-lille.fr/shrec2017-hand/)
2. [Gupta P, Kautz K. Online detection and classification of dynamic hand gestures with recurrent 3d convolutional neural networks[C]//CVPR. 2016, 1(2): 3.](https://openaccess.thecvf.com/content_cvpr_2016/papers/Molchanov_Online_Detection_and_CVPR_2016_paper.pdf)