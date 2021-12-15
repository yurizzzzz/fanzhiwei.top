---
title: 车牌识别：Haar特征+Adaboost原理
date: 2021-05-01 21:47:42
img: /images/18.JPG
author: Yuri
top: false
summary: Haar特征和AdaBoost原理介绍
categories: 机器学习
tags: 
   - 机器学习
   - 车牌识别
---
利用OpenCV中的级联分类器训练的Haar+Adaboost分类算法
在这里我们采用了cascade.xml 检测模型——目前效果最好的cascad级联检测模型。然后使用OpenCV的detectMultiscale的方法来对图像进行滑动窗口遍历寻找车牌，实现粗定位。detectMultiscale函数为多尺度多目标检测：多尺度：通常搜索目标的模板尺寸大小是固定的，但是不同图片大小不同，所以目标对象的大小也是不定的，所以多尺度即不断缩放图片大小（缩放到与模板匹配），通过模板滑动窗函数搜索匹配；同一副图片可能在不同尺度下都得到匹配值，所以多尺度检测函数detectMultiscale是多尺度合并的结果。

Haar+Adaboost原理详解：
1.Haar特征计算
最原始的Haar-like特征在2002年的《A general framework for object detection》提出，它定义了四个基本特征结构，如下A，B，C，D所示，可以将它们理解成为一个窗口，这个窗口将在图像中做步长为1的滑动，最终遍历整个图像。
![](https://img-blog.csdnimg.cn/2020031701205972.png)
在基本的四个haar特征基础上，文章《An extended set of Haar-like features for rapid object detection》对其做了扩展，将原来的4个扩展为14个。这些扩展特征主要增加了旋转性，能够提取到更丰富的边缘信息。
![](https://img-blog.csdnimg.cn/20200317012123704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
haar特征就是一些矩形区域，同一个类型的矩形区域不管放大多少，黑白面积比都不变。用一个类型的haar放大N倍，盖住原图像中的目标区域，把haar特征白色区域盖住的像素点的值的和减去该haar特征黑色区域的像素点的值的和得到的结果作为haar特征值。
比如如下计算举例：
![](https://img-blog.csdnimg.cn/20200317012157173.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
总而言之我给出这样的解释：将上面的任意一个矩形放到车牌区域上，然后，将白色区域的像素和减去黑色区域的像素和，得到的值我们暂且称之为车辆特征值，如果你把这个矩形放到一个非车牌区域，那么计算出的特征值应该和车牌特征值是不一样的，而且越不一样越好，所以这些方块的目的就是把车牌特征量化，以区分车牌和非车牌。
在计算Haar特征的过程中还有一个很重要的就是积分图，所谓积分图可以理解为计算加速器。在原来的Haar特征计算公式中如果对于一个矩形小窗口去遍历一整张图像然后计算出特征值，要知道这计算量可能会相当大，因此利用积分图的方法可以大大加速这过程。积分图即只需遍历一次一整张图像，积分图的构造方式是位置（i,j）处的值ii(i,j)是原图像(i,j)左上角方向所有像素的和。这样就可以把原本计算两个矩阵像素和的差值变成三步运算。
![](https://img-blog.csdnimg.cn/20200317012218668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
2.Adaboost分类
Adaboost全称是Adaptive Boosting自适应增强，它是由弱学习联合形成强学习的算法，前提是弱学习的能力是相互补充的不可是能力相类似的弱分类器。
对于弱分类器就是基于上述的Haar特征计算得出
首先对所有样本图片计算其中一种Haar特征
其次统计正负样本的Haar特征均值
然后找到介于正负样本均值之间的一个阈值，使用此阈值来区分正负样本
最后对所有Haar特征都找到这个最佳阈值，所有特征投票，提高分类效果
![](https://img-blog.csdnimg.cn/20200317012244410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
由上述步骤就可以得到一个初步的弱分类器，接下来就是提高被误判样本的权重，然后再训练得到第二个分类器，然后使用第一二个分类器加权对样本分类并根据结果重新计算权重并训练第三个分类器，以此类推直到第N个分类器的错误率小于所设定的值为止。到此为止就可以完成车牌和非车牌的分类。

以上就是cascade.xml 检测模型的初步检测原理。之后通过detectMultiscale多尺度检测函数实现车牌的最终定位抓取。（Cascade.xml的创建需要准备大量的车牌图像和非车牌图像然后就可以创建模型）
