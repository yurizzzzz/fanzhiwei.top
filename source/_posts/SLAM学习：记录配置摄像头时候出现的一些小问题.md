---
title: SLAM学习：记录配置摄像头时候出现的一些小问题
date: 2021-05-01 21:18:24
img: /images/9.JPG
author: Yuri
top: false
summary: 记录在给ROS小车仿真上安装测试摄像头时遇到的问题
categories: SLAM
tags: 
   - ROS
   - SLAM
---
## 前言
本文是关于笔者在为自己建立的模型上添加摄像头以便为了后续的视觉开发做准备，在配置摄像头的时候遇到了一些小问题，这里本文也只是为了记录而已，可能文章略简，请谅解（本文摄像头使用的是Kinect摄像头）

**一、问题一描述**
![](https://img-blog.csdnimg.cn/20200622001437447.png)
在配置好一切摄像头应该配置的文件之后，运行在gazebo中显示模型的launch文件，出现了以上错误。
解决方法：
1、可能是urdf文件书写有错误，要注意urdf文件中不能加中文注释，第一行中不能加注释，打开urdf文件删掉相关注释即可。
2、安装joint-state-publisher-gui

```
sudo apt-get install ros-kinetic-joint-state-publisher-gui

```

**二、问题二描述**
![](https://img-blog.csdnimg.cn/20200622001915754.png)
在一个自己已经搭建好的world中加载我的机器人模型，发现爆出警告机器人模型已存在，并且显示出来的机器人模型是旧版的不带摄像头的模型。
原因：因为我在搭建模型的时候是在机器人模型已经存在在空世界的环境下搭建所以创建出来的world自带了model所以在构建world的时候一定要把模型删去。
解决方法：
重新在无模型的空世界下搭建world并保存。

**三、参考文献**
[https://blog.csdn.net/qq_40081208/article/details/101533221](https://blog.csdn.net/qq_40081208/article/details/101533221)
[https://blog.csdn.net/liuyuekelejic/article/details/105476063](https://blog.csdn.net/liuyuekelejic/article/details/105476063)

