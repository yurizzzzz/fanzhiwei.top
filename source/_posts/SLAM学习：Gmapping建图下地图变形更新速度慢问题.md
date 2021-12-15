---
title: SLAM学习：Gmapping建图下地图变形更新速度慢问题
date: 2021-05-01 21:22:41
img: /images/13.JPG
author: Yuri
top: false
summary: 在ROS上仿真实现Gmapping建图算法时遇到激光雷达扫描建图成像慢的问题
categories: 建图算法
tags: 
   - 建图算法
   - SLAM
---
**一、问题描述**
首先，我在用Gmapping建图的时候发现扫描出来的地图更新速度很慢然后还有就是地图的完整性不高容易变形扭曲，我们在一般在路径规划前需要用适当的建图算法把地图创建出来然后保存方可进行全局路径规划，但是如果地图创建出来扭曲完整性不高那么在后面的路径规划中也更谈不上规划，问题如下图所示
![](https://img-blog.csdnimg.cn/20200521141320563.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**二、问题解决**
其实主要是修改Gmapping的配置文件，因为官方的配置文件不可以直接用一定要视具体情况而作出修改
在gmapping.launch文件中修改

```
<launch>
    <arg name="scan_topic" default="scan" />

    <node pkg="gmapping" type="slam_gmapping" name="slam_gmapping" output="screen" clear_params="true">
        <param name="odom_frame" value="odom"/>
        <param name="map_update_interval" value="0.1"/>   <!--雷达更新速度-->
        <!-- Set maxUrange < actual maximum range of the Laser -->
        <param name="maxRange" value="12.0"/>     
        <param name="maxUrange" value="10.0"/>         <!--雷达扫描最大距离-->>
        <param name="sigma" value="0.05"/>
        <param name="kernelSize" value="1"/>
        <param name="lstep" value="0.05"/>
        <param name="astep" value="0.05"/>
        <param name="iterations" value="5"/>
        <param name="lsigma" value="0.075"/>
        <param name="ogain" value="3.0"/>
        <param name="lskip" value="0"/>
        <param name="srr" value="0.01"/>
        <param name="srt" value="0.02"/>
        <param name="str" value="0.01"/>
        <param name="stt" value="0.02"/>
        <param name="linearUpdate" value="0.05"/>
        <param name="angularUpdate" value="0.436"/>
        <param name="temporalUpdate" value="-1.0"/>
        <param name="resampleThreshold" value="0.5"/>
        <param name="particles" value="80"/>
        <param name="xmin" value="-1.0"/>
        <param name="ymin" value="-1.0"/>
        <param name="xmax" value="1.0"/>
        <param name="ymax" value="1.0"/>
        <param name="delta" value="0.05"/>
        <param name="llsamplerange" value="0.01"/>
        <param name="llsamplestep" value="0.01"/>
        <param name="lasamplerange" value="0.005"/>
        <param name="lasamplestep" value="0.005"/>
        <remap from="scan" to="$(arg scan_topic)"/>
    </node>
</launch>
```
上面的内容主要是修改那两个注释的地方，最主要的就是更新速度，因为更新速度一提升，建图的完整性也会跟着提高很多，下面是做过修改后的地图
![](https://img-blog.csdnimg.cn/20200521142021140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**三、总结**
给出一些参考的文献和文章
[参考文献](https://blog.csdn.net/qq_42263553/article/details/100587024)


