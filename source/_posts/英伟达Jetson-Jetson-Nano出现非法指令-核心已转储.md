---
title: 英伟达Jetson:Jetson Nano出现非法指令(核心已转储)
date: 2021-07-25 15:11:29
img: /images/1.png
author: Yuri
top: false
summary: 在使用Jetson Nano开发板的过程中运行Python文件终端报错出现的错误提示,后来在终端中输入python3进入python环境去任意import一个库除了python的内置库(比如:sys、os等等)都会直接报错出现非法指令(核心已转储)，因此一度陷入了困境，在网上查找资料怎么也找不到相关的解决办法，刚开始无奈的我只好重装系统，在装好系统后安装完代码相关要求的一些库后运行python代码文件就又开始报出相同的错误。
categories: 深度学习
tags: 
   - 深度学习
   - 边缘AI
   - Jetson
---
## 前言
在使用Jetson Nano开发板的过程中运行Python文件终端报错出现的错误提示,后来在终端中输入python3进入python环境去任意import一个库除了python的内置库(比如:sys、os等等)都会直接报错出现非法指令(核心已转储)，因此一度陷入了困境，在网上查找资料怎么也找不到相关的解决办法，刚开始无奈的我只好重装系统，在装好系统后安装完代码相关要求的一些库后运行python代码文件就又开始报出相同的错误。

## 问题分析
> 首先介绍一下**问题出现的背景**，这里只代表本人的问题背景，问题背景是我在安装好适用在Jetson上的Pytorch后再安装了pytorch-lightning库，至此原因就是在于这个pytorch-lightning库，因为这个pytorch-lightning库有一个要求的依赖库那就是numpy>=1.16.4，但是在我们给Nano刷系统的时候系统自带了一些必要的库其中包括numpy1.13.1，这个numpy库对于Jetson来说很重要，因此这里就会产生一个冲突，安装pytorch-lightning库的时候硬是再下载了最新的numpy1.19.1，这里就是问题的关键，系统内置的numpy1.13.1没有被删掉而又同时存在两个版本的numpy，因此系统其他依赖于numpy的库(比如cv2、pytorch-lightning等)在被调用的时候就会发生了错乱导致python3报错出现**非法指令核心已转储**；  

> 这里先打住上面的问题，现在先谈论一下为什么原有的numpy在没被删除的情况下依旧可以安装新的numpy，因为在给Jetson Nano刷好系统的时候系统里面会自己已经安装了很多库（比如：cv2、scipy、numpy、time等等），这些库其实也都是第三方库，但这些库的所在位置是在`/usr/lib/python3/dist-packages/`这个路径下，而一般按照刷完系统的流程我们会给Nano安装上pip3（顺带说一下，Jetson Nano自带python3.6和python2.7），python2.7基本应该是不会去用的，因此需要在python3的环境下安一个pip3，后面的话安装其他库都是通过pip3来安装，而pip3安装的第三方库的存储路径都是在```/home/username（这里指本机的名字）/.local/lib/python3.6/site-packages/```里面，因此这就是为什么可以同时安装两个版本numpy的原因，而在python导入包的时候即import的时候，import的机制就是搜索sys设置的路径下是否存在相关包，而这时候sys.path显示出两个路径下都有numpy而且还是不同版本因此产生了冲突，显示import的路径的方法是`python3 ；import sys ；print(sys.path)`即可显示

> 说了这么多以上就是问题的分析，基本就可以总结问题的来源就是numpy的冲突问题，而numpy又给很多其他常用的库提供资源因此numpy一出问题大家就都出问题。

## 解决方法
> 这里给出的解决方法不一定准确仅供参考：通过`python3 -m pip uninstall numpy`即可卸载掉最新版的numpy然后保留numpy1.13.1，这样就不会报错了，当然可能有同学们会问到，那把内置的numpy1.13.1删掉再安装最新的不就可以了吗，想法是这样是没错但是其他库对numpy的版本也有限制，比如卸载掉内置的numpy1.13.1装上最新的numpy1.19.1，然后再去import其他库依旧报错因为，其他库还是找不多符合要求的numpy，这里我尝试了卸载掉内置numpy1.13.1版本然后安装numpy1.16.4版本是可以的目前没有报出其他的错误

## 总结
这里在Jetson下出现**非法指令(核心已转储)** 的原因也就是内置库的版本冲突，这里仅代表个人的问题背景，不代表这是唯一的问题根源，但是如果出现类似的情况实在找不多方法可以首先尝试去看看是不是因为**版本冲突**导致的**非法指令(核心已转储)** 。
