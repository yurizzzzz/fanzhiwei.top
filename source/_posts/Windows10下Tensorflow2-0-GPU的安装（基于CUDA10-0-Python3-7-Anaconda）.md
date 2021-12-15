---
title: Windows10下Tensorflow2.0-GPU的安装（基于CUDA10.0+Python3.7+Anaconda）
date: 2021-05-01 21:50:58
img: /images/11.png
author: Yuri
top: false
summary: Tensorflow是当今深度学习很流行的一个框架，它是由谷歌开发的深度学习框架到现在已经发布到了TF2.0版本了。TF的安装有两个版本一个是CPU版另一个是GPU版。当然GPU上运行TF的速度自然比CPU会快，但是自然它的安装也比CPU版要麻烦。CPU版的TF的安装十分的简单，这里当然不作叙述，本文主要是想记录下自己安装GPU版中遇到的一些问题和坑。
categories: Tensorflow
tags: 
   - Tensorflow2
   - GPU
---
Tensorflow是当今深度学习很流行的一个框架，它是由谷歌开发的深度学习框架到现在已经发布到了TF2.0版本了。TF的安装有两个版本一个是CPU版另一个是GPU版。当然GPU上运行TF的速度自然比CPU会快，但是自然它的安装也比CPU版要麻烦。CPU版的TF的安装十分的简单，这里当然不作叙述，本文主要是想记录下自己安装GPU版中遇到的一些问题和坑。

一、首先安装GPU版本的Tensorflow第一步就是检查自己的电脑配置够格不，在官网有配置要求[TF官网](https://tensorflow.google.cn/install/gpu)，我的笔记本电脑显卡是NVIDIA的GTX 1050。如何查看自己电脑显卡配置就是在桌面右击选择NVDIA控制面板然后就可以看到自己的电脑显卡是什么。显卡一定要是NVDIA的AMD的不行，因为在后续要安装CUDA.

二、安装CUDA10.0.CUDA10.0就在官网上下载即可[CUDA官网](https://developer.nvidia.com/cuda-10.0-download-archive?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exelocal)如下图选择
![](https://img-blog.csdnimg.cn/20200206225023985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
下载完成后就会慢慢的出现一个安装CUDA的程序如下图，点击自定义安装(高级)
![](https://img-blog.csdnimg.cn/20200206225250251.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
本人电脑已有VS2017故不再装与VS有关的配置
接下来就是安装了，就选择默认路径了不要再去改到其他地方，我看网上说改到其他路径可能会出现问题，当然我没有去试过。
![](https://img-blog.csdnimg.cn/20200206225656147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
然后CUDA10.0就安装完毕了，然后可能你会看到网上还有什么配置环境变量什么的其实最后人家软件都帮你自动配置好了根本就不用在自己配置了

三、CUDNN的安装。CUDNN同样也是去官网上去下载然后需要保存到CUDA当时的安装路径下。先给出下载地址[CUDNN下载地址](https://developer.nvidia.com/rdp/cudnn-archive)这里要选择对应CUDA10.0版本的cudnn。下载好之后呢你要把这个zip文件解压到这里
![](https://img-blog.csdnimg.cn/2020020623024432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
这里的cudnn文件夹是提前自己创建的，然后解压到这里面来。这个CUDA路径就是当时安装CUDA的时候我说的默认路径，如果你选的是默认路径跟着我走就是对的。接下来呢就是需要把解压后的文件夹里的三个小文件复制到CUDA里面。
![](https://img-blog.csdnimg.cn/20200206230527420.png)
这是官网的教程说明，应该很显而易懂。（解压后的文件夹名字就是cuda）
这里附上官网安装cudnn教程[cudnn官方教程](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#install-windows)
到这里TF2.0的铺垫安装基本完成了接下来就是TF2.0的安装

四、Tensorflow2.0-GPU的安装
这里我们用国内清华源下载比较快，如果不用的话可能会超级无敌慢

```
pip install tensorflow-gpu==2.0.0 -i https://pypi.tuna.tsinghua.edu.cn/simple
```
接下来就是测试我们的Tensorflow2.0了
打开Pycharm（或者是Jupyter Notebook）

```
import tensorflow as tf	
version = tf.__version__	
gpu_ok = tf.test.is_gpu_available()	
print("tf version:",version,"\nuse GPU",gpu_ok)
```
输出

```
tf version: 2.0.0	
use GPU True
```

当然这里如何判断是否代码真的在GPU上面跑，执行以下代码

```
import tensorflow.compat.v1 as tf

tf.disable_v2_behavior()


a = tf.constant([1.2,2.3,3.6], shape=[3],name='a')
b = tf.constant([1.2,2.3,3.6], shape=[3],name='b')

c = a+b
session = tf.Session(config=tf.ConfigProto(log_device_placement=True))
print(session.run(c))
```
输出这样就是GPU

```
Device mapping:
/job:localhost/replica:0/task:0/gpu:0 -> device: 0, name: Quadro M5000, pci bus id: 0000:01:00.0
add: (Add): /job:localhost/replica:0/task:0/gpu:0
b: (Const): /job:localhost/replica:0/task:0/gpu:0
a: (Const): /job:localhost/replica:0/task:0/gpu:0
[2.4 4.6 7.2]
```

可能会刷到说用import tensrflow as tf 然后tf.Session（）做测试，但是会报错说没有modelSession那是因为TF到2.0后的一小改动它的Session在tf.compat.v1里面了。
