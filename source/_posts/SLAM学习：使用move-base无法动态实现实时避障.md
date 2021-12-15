---
title: SLAM学习：使用move_base无法动态实现实时避障
date: 2021-05-01 21:25:05
img: /images/14.JPG
author: Yuri
top: false
summary: 在ROS上仿真实现自动避障和路劲规划算法时小车无法实现实时避障
categories: SLAM
tags: 
   - SLAM
---
## 前言
首先我使用的机器人模型是自己用solidworks制作然后转出为urdf模型的，然后在我用AMCL+Move_base算法的时候发现机器人无法躲避实时障碍物，也就是当你突然放一个物体在机器人面前，激光雷达是可以扫描到前方有物体而且会在rviz中的显示红色的点云信息但是却没有膨胀区域（也就是Move_base功能包中所设定的一个安全+可能危险+危险区域蓝色的一块）

**一、问题描述**
首先呈上问题的图片
![](https://img-blog.csdnimg.cn/20200521102247545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
可以看到上面的情况，在小车的面前有个正方体，但是却无法描述它的膨胀区域，最刚开始的时候我一直以为是我的move_base功能包出现了问题然后我就重新的去安装一遍功能包然后发现还是没有用，但是很奇怪的是当我用别人的一个小车模型但是同样的move_base包的时候却意外的发现是可以出现膨胀区域，那这下子问题就是我的小车模型上了。

**二、问题解决**
在我努力排除可能问题的努力下，终于发现问题所在，那就是我的激光雷达所设置的一个水平高度比较高，然后又因为move_base的配置文件costmap_common_params.yaml里面有一个设置障碍物最小和最大高度的选项；很巧的是两个错开了，就是雷达所在高度扫描过去是可以扫描到但是我move_base配置文件里面设置的识别到的障碍物高度如果在那个范围就直接过滤掉，所以最终原因就是costmap_common_params.yaml中的max_obstacle_height和min_obstacle_height参数设置问题，一般的min_obstacle_height设置为地面高度也就是0,max_obstacle_height设置成高于机器人的高度（我就是设置太低了引发的问题），至此就可以解决问题。
这是修改之后的图片
![](https://img-blog.csdnimg.cn/20200521104914602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**三、总结**
这里要推荐给大家的参考网站
[ROS_wiki](http://wiki.ros.org/move_base/)
[Costmap_2d](https://wiki.ros.org/costmap_2d)
[obstacle层参数配置](https://www.ncnynl.com/archives/201708/1920.html)
