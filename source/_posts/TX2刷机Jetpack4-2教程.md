---
title: TX2刷机Jetpack4.2教程
date: 2021-05-01 21:56:03
img: /images/8.png
author: Yuri
top: false
summary: TX2刷机是一般入手TX2后比较常做的事情，因为一般刚买来的TX2因为内置的Ubuntu版本和一些安装的包都比较旧所以一般我们会选择去通过刷机刷到较新的版本然后获得更新的包和库。首先刷机你需要一台安装有Ubuntu系统或装有Ubuntu虚拟机的电脑，这里不讲如何装Ubuntu双系统或虚拟机，网上教程很多每个人电脑都不一样。（这里我自己的电脑装的是Ubuntu16.04+Win10双系统没有装虚拟机所以这里我不讲虚拟机的刷机教程，还有Ubuntu的版本不是问题不管是16.04还是18.04都一样，个人认为电脑只是个刷机工具媒介而已，在Ubuntu上下载好SDK后通过USB转micro-usb传到TX2上。）接下来我们开始刷机过程。
categories: NVIDIA
tags: 
   - NVIDIA
   - Linux
---
**TX2刷机Jetpack4.2刷机教程**
文章参考以下链接：
[https://blog.csdn.net/zt1091574181/article/details/88847775](https://blog.csdn.net/zt1091574181/article/details/88847775)
[https://blog.csdn.net/YiYeZhiNian/article/details/94407065](https://blog.csdn.net/YiYeZhiNian/article/details/94407065)
[https://blog.csdn.net/qq_41587270/article/details/97623350](https://blog.csdn.net/qq_41587270/article/details/97623350)
这几篇都还可以。
这几篇是我结合起来后自己再去写一篇关于自己所安装的过程，如果其中有侵权的地方请联系我，谢谢！

TX2刷机是一般入手TX2后比较常做的事情，因为一般刚买来的TX2因为内置的Ubuntu版本和一些安装的包都比较旧所以一般我们会选择去通过刷机刷到较新的版本然后获得更新的包和库。

首先刷机你需要一台安装有Ubuntu系统或装有Ubuntu虚拟机的电脑，这里不讲如何装Ubuntu双系统或虚拟机，网上教程很多每个人电脑都不一样。（这里我自己的电脑装的是Ubuntu16.04+Win10双系统没有装虚拟机所以这里我不讲虚拟机的刷机教程，还有Ubuntu的版本不是问题不管是16.04还是18.04都一样，个人认为电脑只是个刷机工具媒介而已，在Ubuntu上下载好SDK后通过USB转micro-usb传到TX2上。）接下来我们开始刷机过程。

一.下载NVIDIA SDKManager
     从官网上下载[（官网地址）](https://developer.nvidia.com/embedded/jetpack)
     ![](https://img-blog.csdnimg.cn/20191111215830717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
进入后点击SDK Manager下载，然后会跳到一个注册界面，按要求注册一个账号就行，账号要记得因为后面要用到。然后会得到一个.deb的文件包，然后进入/home/下载里面找到那个包直接双击打开就行，可能需要等一些些时间，然后出现下面画面点击安装。
![](https://img-blog.csdnimg.cn/20191111220430461.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
安装好之后可以看到下面的界面，输入刚刚创建的NVIDIA账号密码
![](https://img-blog.csdnimg.cn/20191111220625173.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
然后进入STEP1，选择下Target Hardware中的TX2记得选啊不要选到别的去了。然后system要选Linux的Jetpack4.2
![](https://img-blog.csdnimg.cn/20191111220922365.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
点击Continue进入STEP2，什么都不用勾选直接点击继续。（其实这里多说一句话，如果你刷的是Jetpack4.2.2的话它是比4.2多出了个Deepstream和Tensorflow的包，但是我其实第一次就是装4.2.2的但后面就是安装过程一半老是出现error或者是卡进度条一直不动。于是乎我就索性都退出去安装4.2版本的然后比较顺利。）
接下来就是STEP3安装过程
![](https://img-blog.csdnimg.cn/2019111122135130.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)

这个过程极其漫长然后很艰辛中间可能会出现安装失败或者是卡进度条就是一直进度不动的那种，而且关于网络的连接也是个问题，我尝试用过WI-FI，网线，手机热点，但我看网上说手机热点最好其实我觉得都差不多啦不过热点确实好那么一丢丢比较稳定点，还有就是网上也有人说换个源可以下的快一点但是我也试过了，其实我觉得没差还不如不换，当时换源的时候还出了事故。（不管是下载进度还是安装进度中出现问题，不要放弃就退出去重新再登陆一下再点击一遍过去，下载过的会保存不会重复下载）
如果有人要尝试换源我也给出换源的方法，换的是阿里源。
1.备份
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backu
```

2.修改添加阿里源

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backu
```
3.出现一个文件然后在最底下添加以下代码

```
# 阿里云
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```
当你成功下载完以后恭喜你！
下载好之后，用USB转micro-usb线将电脑和TX2相连接，其中USB接电脑Micro-usb接TX2，这边记住要用买TX2时官方送的线，我当时试了好几根线SDK就一直报错说烧不进去，这里我展示下官方的线。（两个头上有绿色标志的）
![](https://img-blog.csdnimg.cn/2019111122295794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
接下来就是要把TX2进入Recovery模式，操作方法如下：
1.先把电源要拔掉然后再直接插上
2.按下S4 power键一下后松开开启
3.紧接着马上一直按住S3键Force Recovery不放
4.还是按住S3不放的同时赶紧按一下S1键Reset后松开
5.然后心中默数两秒后松开S3键
然后就进入了Recovery模式此时接TX2的显示屏上是一团黑的，正常。
这时候在进入Recovery和USB线接上的情况下，在电脑端的终端输入lsusb看看有没有NVIDIA crop，好像是会出现在中间一行，反正就出现的几行找一下有没有这个，有的话就是电脑和TX2连接上了。

![](https://img-blog.csdnimg.cn/201911112244444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
如果出现这个记得不要选择自动，要选择另外一个手动

在刚刚的STEP3下载好后就是会提示你要不要flash你的TX2你就顺着提示来点击就行然后flash进去，这里也要等的挺长一段时间的。在flash过程其实也会出现erro或卡进度条情况，一样的重来，反正系统有存档不会重复下载的。

我记得flash后TX2的显示屏会出现界面然后提示你一步步开始创建新的用户名啥的，反正很简单跟着提示走，对了在创建用户名和密码时候要记住因为在电脑端会让你输入TX2的用户名和密码然后进行核对匹配。
最后应该就没有了，最后安装好之后就会出现这样
![](https://img-blog.csdnimg.cn/20191111224947799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
然后这就是整个刷机过程了，可能我写的不是很好但是希望其他人能够少踩点坑。不过踩坑也挺好的加深印象。
顺带附上刷机后新的TX2如何小风扇散热。
Jetpack4.2以上的打开散热扇需要

```
cd /usr/bin/
sudo ./jetson_clocks
```



















