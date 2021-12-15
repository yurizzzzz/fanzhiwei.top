---
title: SLAM学习：Solidworks2018导出小车URDF模型下遇到的问题
date: 2021-05-01 21:27:27
img: /images/15.JPG
author: Yuri
top: false
summary: Solidworks2018导出小车URDF模型后在ROS上显示时小车模型混乱
categories: SLAM
tags: 
   - SLAM
   - URDF
   - Solidworks
---
## 前言
这是我在用Solidworks2018来自己画一个autolabor小车装配体然后转成urdf文件然后在ROS下显示出模型时遇到的问题，下面是我的装配体模型。
![](https://img-blog.csdnimg.cn/20200510010801559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**一、问题**
1、转成urdf模型的步骤我不想多说自己可以看古月居的视频或者百度上面自己搜索或者官网上教程这很简单不是很难。在导出后得到一个功能包，然后把功能包转移到Ubuntu下的ROS工作空间里面的src文件夹下，之后进行catkin_make等基本操作，但当我运行display.launch文件的时候出现了以下情况
![](https://img-blog.csdnimg.cn/20200510011044908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
这个情况纯属是我自己的粗心导致可以看到很明显的error提示，我有一个joint重复了，后来发现是因为我在soildworks里面命名的joint确实命名重复了，这个得返回solidworks里面重新配置名字然后导出新的功能包就好了

2、这个问题算是比较麻烦也是不那么好解决的，在解决上面的问题之后本以为已经可以正常显示模型了但是没有想到的是模型出现了下图这样的情况——四个轮子只有一个是正常的
![](https://img-blog.csdnimg.cn/20200510011307824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
经过不断的查找资料，排除各种情况的操作下，终于发现了问题所在，竟是由于solidworks2018转urdf的插件问题，就是不知道为什么如果在你的装配体中如果出现相同的多个零件，那么在转urdf的时候自动配置会出现异常——即仅会保留相同零件中的一个零件。既然问题已经知道了，那么其实很好解决那就是回到solidworks下重新画四个不一样的轮子然后装配上去即可，至此解决了问题。
![](https://img-blog.csdnimg.cn/20200510011606658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
3、通过以上的经历，感觉差不多了，但是还是有小毛病就是，原来在solidworks里面已经配色好的模型但是在urdf里面颜色已经全部丧失，后来我分析了一波，我个人认为是urdf中的rgb（0-1）颜色范围和我们平时所定义的rgb（0-255）范围不是一致因此才有这样情况，这也好解决只要进入urdf文件夹下去配置一下color这个属性就可以，至于rgba的值大家可以在网上查到资料

**二、结语和总结**
在整个过程中利用solidworks进行模型构建能够更加真实的仿真模拟，相比于自己用xml语言一个字一个字去敲真的方便太多了，但是这其中难免会有一些困难等。