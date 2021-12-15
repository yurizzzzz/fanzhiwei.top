---
title: 论文阅读：Unprocessing Images for Learned Raw Denoising
date: 2021-05-01 20:34:53
img: /images/3.JPG
author: Yuri
top: false
summary: CVPR2019去噪论文阅读
categories: 论文阅读
tags: 
   - ISP
   - 去噪
   - 神经网络
---
## 前言
本文主要是对《Unprocessing Images for Learned Raw Denoising》论文进行部分翻译解读，如果有观点不对或者错误的地方还望谅解并指出。

## 一、介绍
首先谈谈本文的一个基线，本文的目的是通过sRGB模拟转换成RAW图输入神经网络中进行降噪然后再转换回sRGB图像形成最终降噪后的图像。本文的最终目的是降噪但是亮点（或核心）不在降噪而是在于sRGB图模拟转换成RAW图，通过阅读该论文可以看出论文中有相当一大部分是在说“unprocess”也就是sRGB形成RAW图的过程，差不多占到快一半的部分吧。以下是整个论文的算法架构。
![](https://img-blog.csdnimg.cn/20201201135230967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)

## 二、Unprocess
输入的sRGB图像总共是三通道，经过Unprocess后输出的图像总共是四通道的图像，具体这四通道的图像是什么样呢？其实就是Bayer拜耳图像，可以通过百度进一步了解Bayer图像，这里只是简单介绍Bayer图像。输出的四通道图像分别是R,Gr，Gb，B这四个通道矩阵，Bayer格式的图像其实就是相机内部最原始的图像，这也是为什么要输出四通道的原因，如下图所示可以看出Bayer图像中RGB阵列是有规律的进行排布。可以看出偶数行输出是GBGB而奇数行输出是RGRG（第一列或第一行是0行/列所以作为偶数行/列），这也是论文中转换成Bayer RGB图像的基础，取偶数行和奇数行的GBGB和RGRG。
![](https://img-blog.csdnimg.cn/20201201141556309.png)
**1、Invert Tone Mapping色调映射的反变换**
所谓色调映射就是调整图像的整体灰度，使得图像更加均衡并且使原有信息更加清楚的表达出来这里借一张图来说明解释
![](https://img-blog.csdnimg.cn/20201201142830835.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
而对色调映射进行反变换后，显而易见图像的整体亮度和清晰度也会随之减小，该论文中使用的色调映射算法公式如下
![](https://img-blog.csdnimg.cn/20201201142942741.png)
**2、Gamma Decompression伽马解压**
这里其实就是伽马矫正公式的反变换，伽马矫正也是调整图像整体灰度分布和亮度的一个操作，伽马矫正的公式和伽马矫正反变换的公式如下
![](https://img-blog.csdnimg.cn/20201201143322888.png)![](https://img-blog.csdnimg.cn/20201201143341949.png)
**3、Color Correction色彩校正**
相机应用一个3x3颜色校正矩阵(CCM)来将自己的相机空间RGB颜色测量转换为sRGB值。数据集由四个摄像头组成，每个摄像头在进行色彩校正时使用自己的固定CCM。为了生成能够推广到数据集中所有相机的合成数据，我们对这四种CCM的随机凸组合进行采样，对于每一幅合成图像，我们应用采样CCM的逆来撤销颜色校正的效果。
**4、White Balance白平衡**
白平衡主要的作用是，由于在不同的色温，光照下图像会出现较大的偏色因此需要白平衡算法进行处理消除影响。白平衡的算法有很多种，本文中使用的白平衡算法主要是通过RGB三通道的值乘上各自的增益即可。本文中Red的增益在[1.9,2.4]，Blue的增益在[1.5,1.9]，当G增益>1，x>t时
![](https://img-blog.csdnimg.cn/2020120115064133.png)
**5、Demosaicing去马赛克**
这里其实就是RAW图中Bayer RGB格式和sRGB格式的转换，传统相机传感器中的每个像素都由一个红色、绿色或蓝色滤光片覆盖，滤光片按拜耳模式排列R-Gr-Gb-B，转换方法在提头中有提到，sRGB向Bayer的转换只要提取相应偶数和奇数行/列即可

## 三、UNet神经网络结构
![](https://img-blog.csdnimg.cn/20201201151015985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
如图所示本文中的网络结构清晰可见，其中下采样使用池化实现，上采样使用插值实现，最后网络输出的图像和原始图像进行相加得到最终降噪RAW图，其中损失函数的计算是在process成sRGB图像后进行计算而非在RAW图上进行计算

## 四、总结
在原论文中，作者谈到为什么要进行RAW图像转换，其根本目的是为了验证由于现在大多图像都是由人工合成不真实的图像训练数据会影响到算法的准确性，因此进行模拟相机RAW图再进行数据训练会提高结果的准确性和可信度。

## 五、参考文献
[1][Unprocessing Images for Learned Raw Denoising原文翻译](https://blog.csdn.net/u013049912/article/details/84997826)
[2][Unprocessing Images for Learned Raw Denoising论文解读](https://chenjunkai.blog.csdn.net/article/details/106882131)
[3][Bayer图像格式介绍](https://blog.csdn.net/qingzhuyuxian/article/details/82965054)


