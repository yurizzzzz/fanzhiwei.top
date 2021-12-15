---
title: SLAM学习：JetsonTX2工控机的准备工作
date: 2021-05-01 21:13:54
img: /images/8.JPG
author: Yuri
top: false
summary: 使用NVIDIA的TX2作为工控机安装在ROS小车上
categories: SLAM
tags: 
   - ROS
   - SLAM
   - NVIDIA
---
## 前言
笔者已在自己的PC机上的Ubuntu系统上已经全部模拟仿真好Autolabor无人小车实现SLAM，视觉目标检测等功能后，准备把所有的源码移植到在JetsonTX2上面，然后利用TX2连接Autolabor小车来实现真正的无人驾驶功能，接下来这是我在使用TX2的时候记录一些比较麻烦的问题，全篇内容较短。

**一、TX2刷机成Ubuntu16.04LTS系统**
首先我们得明确我们是要在TX2上面用ROS-Kinetic那就必须安装Ubuntu16.04,但是现在已经2020年，Jetpack的版本都更新到哪里去了，现在基本都是支持Ubuntu18.04,但Ubuntu16.04也有只不过不好搞，这里分享一篇我一直参考比较多的一片博客，写的还不错。
[TX2刷机（Jetpack3.3）](https://blog.csdn.net/DeepWolf/article/details/88640937)
当然刷机的时候会遇到问题（比如Jetpack刚开的时候是空白），这一篇博客已经写了我要说的东西了。
[TX2刷机常见问题](https://blog.csdn.net/bluewhalerobot/article/details/81587039)
这里我额外说一句，主机上安装Jetpack的时候你的系统的语言你得改为EN也就是英语，而且后面Jetpack保存路径也要是英文的不要中文否则会出错哦。
[Ubuntu修改系统语言](https://blog.csdn.net/BobYuan888/article/details/88662779)

**二、TX2必须装上CUDA和Cudnn**
首先说明，一定要看清楚JetsonTX2是ARMV8结构也就是Arm64-bit不是AMD64啊，不要乱下载，如果在刷机的时候你的CUDA和Cudnn没安装上没关系，到了TX2上面手动下载
[TX2安装CUDA和CUdnn](https://blog.csdn.net/sunshinefcx/article/details/104024074?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-5.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-5.nonecase)

**三、TX2的Ubuntu不一样**
由于TX2的Ubuntu16.04是建立在它ArmV8架构上，所以有很多地方不一样要小心，比较重要的一个就是当你TX2有了界面后登录进去第一件事一定是换镜像源，不然后面安装一些其他任何软件等会出问题，原因大概是因为arm架构我记得，反正第一件事要换源，至于怎么换源请看
[TX2换镜像源](https://blog.csdn.net/mathlxj/article/details/99626029)

**四、结语和总结**
不管是TX2也好还是其他嵌入式开发平台也好，一定要清楚机子的配置，因为我们平常都用惯了x86/x64的处理器很可能会习惯性的在新的嵌入式板上做错误的操作。

