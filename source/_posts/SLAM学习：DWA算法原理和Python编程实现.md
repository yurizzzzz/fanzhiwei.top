---
title: SLAM学习：DWA算法原理和Python编程实现
date: 2021-05-01 21:20:43
img: /images/11.JPG
author: Yuri
top: false
summary: Python手撕DWA路径规划算法
categories: 路径规划
tags: 
   - 路径规划算法
   - SLAM
---
## 前言
此部分内容是ROS中Move_Base功能包用到的DWA路径规划算法的介绍和实现，下面我将以自己所理解的DWA算法原理内容展示出来，我看过网上和书籍很多资料，它们的描述基本大同小异，对于想要清楚了解或刚接触DWA算法的人来讲真的比较难理解可能要多看那么四五遍或五六遍才能真正理解这也是比较耗费时间的，所以本文在我理解的基础上写的，本文内容如果有不当之处还请谅解，如果有错误之处也请指出，毕竟本文是以我自己所理解的方式去完成的。

**一、DWA算法原理**
首先先给出DWA算法的原理流程图

![](https://img-blog.csdnimg.cn/20200621101602368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)

**首先**我将先讲一讲评价函数的计算，评价函数其实是比较简单的。
1、距离目标点评价函数：距离目标点评价函数这里采用的就是很普遍也很普通的欧式距离算法，但具体是哪两个点的距离呢，目标点是人为一开始给定的，我们已经有了，另一个点就是一个速度的模拟轨迹的最后一个位置，可能看到这里还会比较模糊什么是模拟轨迹的最后一个位置，没关系，这里先说一下评价函数要求对评价函数怎么算心中有个概念即可。
2、速度评价函数：速度评价函数也比较简单，就是计算刚开始人为设定的机器最大线速度值和模拟轨迹的最后一个位置的线速度的差值，总而言之就是两个速度差值就是，这里也是一样先有个概念。
3、轨迹距离障碍物的评价函数：轨迹距离障碍物也比较简单，就是对一条轨迹上每个采样的位置进行循环，再对每个障碍物进行循环，也就是一条模拟轨迹路线上每个位置和每个障碍物之间的欧氏距离如何，然后比较得出一个距离最小的值，当然在循环的过程中要不断判断每个距离是不是小于机器人自身半径，因为如果小于自身半径我们判定机器人已经撞上去了这时候就直接返回。

**其次**，我们要开始讲一下速度采样的问题了，这里我要偷一张图来帮助理解了。
![](https://img-blog.csdnimg.cn/20200621103159659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
蓝色圆圈代表机器人，比如现在机器人正处于如上位置，速度采样是什么意思呢，就是在一个动态范围内选取速度，如上是不是有很多发散出来的线，那就是选取到的速度，这里速度其实就是线速度和角速度组合，那这个速度是要怎么选取呢，其实很简单，那就是规定线速度范围和角速度的范围，然后循环使得线速度和角速度互相匹配组成一个速度。而线速度一开始会设定Vmin——Vmax，然后在实际情况中会有Vc-va*dt——Vc+va*dt（其中Vc是当前线速度，比如刚开始线速度是0后面会随着速度取样慢慢改变，可能这里讲的有些不明白但是看了代码就一定会懂，va是设定的线速度加速度，dt是间隔时间，也就是一条轨迹上隔多久算一次位置等信息直到模拟轨迹时间）；同理角速度也是如此，经过上述后我们得到两个范围Vmin——Vmax和Vc-va·dt——Vc+va·dt这里我们就要取两个范围的交集了，那肯定是两个范围的最小值的取最大，两个范围的最大值取最小，从而形成一个新的范围，不知道这里我有没有讲清楚。

**之后**速度采样完后我们就可以开始计算模拟轨迹上各个点的位置等相关信息了。
位置的计算其实很简单就是如下图计算公式：
![](https://img-blog.csdnimg.cn/2020062110452552.png)
这里的v就是线速度，w就是角速度，这里可能很多人会好奇为什么这里计算位置使用直线方式计算，这是因为我们一条模拟轨迹上所设定的时间间隔很小所以可以近似把它看作直线来计算，同时这也使得代码计算更为方便快捷，那这里我再偷一张图。
![](https://img-blog.csdnimg.cn/20200621105935889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
这里可能大家会看到机器人还有个Yrobot方向速度，大家其实可以不用管，因为一般正常的轮子都是只能实现前后方向的位置改变，但是可能有些人会接触到麦克纳姆轮也就是全向移动轮，它可以做到前后左右位置改变，所以其实这里我们大可不必考虑Y方向速度，如果你真想知道你就在百度上随便搜一篇DWA算法你就会在里面找到相关计算方法，但是这里我并不想阐述计算方法。

**最后**，讲到这里DWA算法基本的东西其实已经全部讲完了，其实这个算法并不会太难，如果看不懂就多读几遍，最后的步骤就是不断地循环循环找最优轨迹并沿着最优轨迹走然后再速度采样再模拟轨迹再选择最优轨迹走，直到到达某个位置和目标点的距离不超过自己设定的范围为止。对了，刚刚上面只讲了评价函数但是我给忘了讲评价分的计算，其实也很简单，最简单的方法就是把三个评价函数计算出来的结果全部相加起来当然这是最简单的计算方法，实际上我们不能这样，因为每个参量有大有小，可能因为某个评价函数的结果过大直接覆盖了其他两个，所以我们还是要进行平滑处理的操作，这里我要再去偷个公式图。
![](https://img-blog.csdnimg.cn/20200621105953621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
这个图里面的head就是距离目标点距离的评价值，dist就是距离障碍物的，velocity就是速度，其实总而言之就是归一化呗，没什么特别深奥的东西，最后的评价分计算公式就是把这归一化后的三个值相加起来，当然可以相应的在每个评价值前面乘上权重系数，也就是偏重于哪个评价值，最后计算公式就是
![](https://img-blog.csdnimg.cn/20200621110256507.png)

**二、DWA算法Python实现代码**
首先说明一下这个代码我是参考给出的标准的DWA算法实现，但是我是在自己完全理解后自己看着DWA公式来写出算法，代码的每一步基本都有注释。
这份代码里面可能大家看到，我没有用权重系数和归一化，这是因为我是在坐标轴上一个小区域直接模拟，所以计算量都不是很大，所以我就没有用到，在真正以后实际情况里，还是要按照公式来做

```
import numpy as np
from math import *
import matplotlib.pyplot as plt

#参数设置
V_Min = -0.5            #最小速度
V_Max = 3.0             #最大速度
W_Min = -50*pi/180.0    #最小角速度
W_Max = 50*pi/180.0     #最大角速度
Va = 0.5                #加速度
Wa = 30.0*pi/180.0      #角加速度
Vreso = 0.01            #速度分辨率
Wreso = 0.1*pi/180.0    #角速度分辨率
radius = 1              #机器人模型半径
Dt = 0.1                #时间间隔
Predict_Time = 4.0      #模拟轨迹的持续时间
alpha = 1.0             #距离目标点的评价函数的权重系数
Belta = 1.0             #速度评价函数的权重系数
Gamma = 1.0             #距离障碍物距离的评价函数的权重系数

#障碍物
Obstacle=np.array([[0,10],[2,10],[4,10],[6,10],[3,5],[4,5],[5,5],[6,5],[7,5],[8,5],[10,7],[10,9],[10,11],[10,13]])
#Obstacle = np.array([[0, 2]])

#距离目标点的评价函数
def Goal_Cost(Goal,Pos):
    return sqrt((Pos[-1,0]-Goal[0])**2+(Pos[-1,1]-Goal[1])**2)

#速度评价函数
def Velocity_Cost(Pos):
    return V_Max-Pos[-1,3]

#距离障碍物距离的评价函数
def Obstacle_Cost(Pos,Obstacle):
    MinDistance = float('Inf')          #初始化时候机器人周围无障碍物所以最小距离设为无穷
    for i in range(len(Pos)):           #对每一个位置点循环
        for j in range(len(Obstacle)):  #对每一个障碍物循环
            Current_Distance = sqrt((Pos[i,0]-Obstacle[j,0])**2+(Pos[i,1]-Obstacle[j,1])**2)  #求出每个点和每个障碍物距离
            if Current_Distance < radius:            #如果小于机器人自身的半径那肯定撞到障碍物了返回的评价值自然为无穷
                return float('Inf')
            if Current_Distance < MinDistance:
                MinDistance=Current_Distance         #得到点和障碍物距离的最小

    return 1/MinDistance

#速度采用
def V_Range(X):
    Vmin_Actual = X[3]-Va*Dt          #实际在dt时间间隔内的最小速度
    Vmax_actual = X[3]+Va*Dt          #实际载dt时间间隔内的最大速度
    Wmin_Actual = X[4]-Wa*Dt          #实际在dt时间间隔内的最小角速度
    Wmax_Actual = X[4]+Wa*Dt          #实际在dt时间间隔内的最大角速度
    VW = [max(V_Min,Vmin_Actual),min(V_Max,Vmax_actual),max(W_Min,Wmin_Actual),min(W_Max,Wmax_Actual)]  #因为前面本身定义了机器人最小最大速度所以这里取交集
    return VW

#一条模拟轨迹路线中的位置，速度计算
def Motion(X,u,dt):
    X[0]+=u[0]*dt*cos(X[2])           #x方向上位置
    X[1]+=u[0]*dt*sin(X[2])           #y方向上位置
    X[2]+=u[1]*dt                     #角度变换
    X[3]=u[0]                         #速度
    X[4]=u[1]                         #角速度
    return X

#一条模拟轨迹的完整计算
def Calculate_Traj(X,u):
    Traj=np.array(X)
    Xnew=np.array(X)
    time=0
    while time <=Predict_Time:        #一条模拟轨迹时间
        Xnew=Motion(Xnew,u,Dt)
        Traj=np.vstack((Traj,Xnew))   #一条完整模拟轨迹中所有信息集合成一个矩阵
        time=time+Dt
    return Traj

#DWA核心计算
def dwa_Core(X,u,goal,obstacles):
    vw=V_Range(X)
    best_traj=np.array(X)
    min_score=10000.0                 #随便设置一下初始的最小评价分数
    for v in np.arange(vw[0], vw[1], Vreso):         #对每一个线速度循环
        for w in np.arange(vw[2], vw[3], Wreso):     #对每一个角速度循环
            traj=Calculate_Traj(X,[v,w])
            goal_score=Goal_Cost(goal,traj)
            vel_score=Velocity_Cost(traj)
            obs_score=Obstacle_Cost(traj,Obstacle)
            score=goal_score+vel_score+obs_score
            if min_score>=score:                    #得出最优评分和轨迹
                min_score=score
                u=np.array([v,w])
                best_traj=traj

    return u,best_traj

x=np.array([2,2,45*pi/180,0,0])                          #设定初始位置，角速度，线速度
u=np.array([0,0])                                        #设定初始速度
goal=np.array([8,8])                                     #设定目标位置
global_tarj=np.array(x)
for i in range(1000):                                     #循环1000次，这里可以直接改成while的直到循环到目标位置
    u,current=dwa_Core(x,u,goal,Obstacle)
    x=Motion(x,u,Dt)
    global_tarj=np.vstack((global_tarj,x))                 #存储最优轨迹为一个矩阵形式每一行存放每一条最有轨迹的信息
    if sqrt((x[0]-goal[0])**2+(x[1]-goal[1])**2)<=radius:  #判断是否到达目标点
        print('Arrived')
        break

plt.plot(global_tarj[:,0],global_tarj[:,1],'*r',Obstacle[0:3,0],Obstacle[0:3,1],'-g',Obstacle[4:9,0],Obstacle[4:9,1],'-g',Obstacle[10:13,0],Obstacle[10:13,1],'-g')  #画出最优轨迹的路线
plt.show()
```
效果展示
![](https://img-blog.csdnimg.cn/20200621195652606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**三、结语和总结**
其实看不懂的东西只要多看几遍多耐心的琢磨就肯定会明白其中的道理，多看几遍真的没有错。只要自己不要自暴自弃就好，以前老师总说不懂的知识多看几遍，我一直不放在心上，长大后才发现老师说的真的是对的，很多事情也是如此，多看多做几遍，简单的事情重复做，重复的事情坚持做！

**四、参考文献**
[https://blog.csdn.net/u011600592/article/details/54613717](https://blog.csdn.net/u011600592/article/details/54613717)
[https://scm_mos.gitlab.io/motion-planner/dwa-local-planner/](https://scm_mos.gitlab.io/motion-planner/dwa-local-planner/)


