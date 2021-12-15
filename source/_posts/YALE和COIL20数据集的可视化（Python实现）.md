---
title: YALE和COIL20数据集的可视化（Python实现）
date: 2021-05-01 21:52:31
img: /images/10.png
author: Yuri
top: false
summary: 使用Python进行YALE和COIL20数据集的可视化操作,本文使用的数据集是YALE_32X32.mat和COIL20.mat数据集，这两个数据集应该是很容易就可以得到的。这里给出两个数据集链接
categories: OpenCV
tags: 
   - OpenCV
---
本文使用的数据集是YALE_32X32.mat和COIL20.mat数据集，这两个数据集应该是很容易就可以得到的。这里给出两个数据集链接
[YALE_32X32](https://pan.baidu.com/s/1611lz6Wt2eCR_yHrC_5yjA)
提取码：94l2
[COIL20](https://pan.baidu.com/s/1iZRZDB42rhIAMWsACoyAWw)
提取码：8cjn

用Python来可视化数据集其实很简单当然也可以用C++或Matlab可视化也可以的，但是本人偏向于喜欢用Python来而且Python的优点很多接口也很多很适合用来做人工智能。可视化数据集没有什么困难的，当然如果是新手来说，可能你首先一定要很清楚什么是图像，图像处理的一些基本概念和内容然后再去操作会更好。其实很早就想把自己半年来学习的东西总结了只不过一直没有时间刚好寒假总算有时间把当时刚开始接触人工智能的一些基本的东西写下来记录下来，算是自己的一个笔记本或日记吧。

先说下图像吧，一张图像是由像素点组成的，平常我们所看到的彩色图像有三个通道RGB三个也就是三原色通道，而黑白图像就只有一个通道，RGB图像其实就是三层图像叠加在一起组成彩色的图像，一种颜色有256个等级为0~255，如果是灰白图像那就只有一层0-255。一般在图像处理中把图像的像素组成一个矩阵，然后用矩阵的方法来处理图像对图像进行操作。其实图像处理我觉得数学真的很重要特别是线性代数，线性代数就是图像处理的基础其次就是高数，有了高数就可以在图像上试一下各种算法等。

接下来上代码：
这是YALE数据集的可视化

```
import numpy as np
import matplotlib.pyplot as plt
from scipy import io
import cv2 as cv

#像numpy，matplotlib，scipy，cv2这些包。直接下一个Anaconda就可以
#Anaconda里面各种包都有，如果你是新手多百度看看什么是包，这些包有什么作用
#遇到不会多百度

x=io.loadmat('C://Users//dell//Desktop//Maching_Learning//YALE人脸数据集可视化//.idea//Yale_32x32.mat')
#载入YALE32mat文件的方法，得到的x是一个字典，可以print一下他的shape看一下里面的属性
data=[]
print(x['fea'].shape)
#数据都存在了fea这个键值里面，print出来发现是165*1024说明有165张
#图片每张图片都有1024个特征也就是1024个像素点

num=x['fea'].shape[0]
#得到总共样本的个数165张图片

#print(x['fea'].shape[0])

for i in range(num):
    img=np.array(1024)#先创建一个一行1024列的数组
    img=x['fea'][i]#存放每张图片数据
    img.shape=32,32#把这1x1024的数组reshape成32x32的矩阵
    img=img.T#矩阵转置
    data.append(img)#把处理过大小的图片存到data列表里面

data=np.array(data)#把列表转成数组矩阵型

out=[0]*15       #我们想要搞出个15行11列的一整张图先处理行上

for i in range(15):  #对每一行进行遍历
   out[i]=data[i*11]  #换行
   for j in range(10):  #对每一列进行遍历
       out[i]=np.hstack((out[i],data[i*11+j+1]))#将每一次的图片合在一起，np.hstack是行方向的合并矩阵

c=out[0]     #上面得到了15个一行11列矩阵现在要把这15行都合在一起，np.vstack讲就是列方向上合并
for i in range(14):
    c=np.vstack((c,out[i+1]))
print(c.shape)

cv.imshow("out",c)
cv.waitKey(0)
```

这是COIL20数据集的可视化

```
import numpy as np
import matplotlib.pyplot as plt
from scipy import io
import cv2 as cv

x=io.loadmat('C://Users//dell//Desktop//Maching_Learning//COIL20数据集可视化//COIL20.mat')
#导入数据集
data=[]

num=x['fea'].shape[0]
#得到数据样本的个数

#print(x['fea'].shape[0])

for i in range(num):
    img=np.array(1024)
    img=x['fea'][i]
    img.shape=32,32
    img=img.T
    data.append(img)

data=np.array(data)

out=[0]*30

for i in range(30):
   out[i]=data[i*48]
   for j in range(47):
       out[i]=np.hstack((out[i],data[i*48+j+1]))

c=out[0]
for i in range(29):
    c=np.vstack((c,out[i+1]))
print(c.shape)


cv.imshow("out",c)
cv.waitKey(0)
```
下面分别是YALE和COIL20的可视化出来图片
![](https://img-blog.csdnimg.cn/20200204214224334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20200204214234404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)