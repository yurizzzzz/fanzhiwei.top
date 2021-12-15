---
title: 深度学习：Pytorch训练数据时问题总结
date: 2021-05-01 20:14:31
img: /images/2.jpg
author: Yuri
top: false
summary: 记录Pytorch训练数据时遇到的问题
categories: 深度学习
tags: 
   - 深度学习
   - Pytorch
---
## 前言
本文是在笔者使用Pytorch深度学习框架训练自己的网络时遇到的一些问题，在此一并记录下来作为学习笔记，本文中的问题主要是Pytorch框架上遇到的一些问题而非单独针对个人网络代码框架上的问题

## 问题一、
**问题**：在使用Pytorch加载训练好的模型文件checkpoint.tar或其它保存类型的模型文件时，如果遇到“xxxxx is a zip archive (did you mean to torch.jit.load()?)”，大概就是这样一回事
**分析**：这里大概意思就是说你这个版本的Pytorch无法读入这个你这个在高版本运行出来保存的模型文件，在高版本的Pytorch的torch.save方法中已经把旧的保存的模型文件方法换成了一种新的基于zip的保存方法，而这时候你的旧版本Pytorch无法读入高版本产生的模型文件；特此说明，自从torch1.6及之后开始，Pytorch就更换了保存模型文件的方法，见下面官方图
![](https://img-blog.csdnimg.cn/20201218210342460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**解决方法**：如果你坚持要用旧版的保存模型方法，OK可以，但是你得在torch.save后面加上_use_new_zipfile_serialization=False，意思就是不使用新的zip保存方法，比如

```
torch.save(model, _use_new_zipfile_serialization=False)
```
## 问题二
**问题**：在训练模型的时候遇到RuntimeError:CUDA out of Memory等等，然后说allocate xxxx MB/GB （but xxxxxxx），总的来说就是你要训练的模型所需要的GPU显存空间不足
**分析**：一般整个训练代码中，耗GPU内存最多的就是网络结构，而网络结构中最耗内存的就是参数和激活函数产生的中间变量等，当然还有可能是你的单个数据图像的过大或者你一次处理的batch_size太大
**解决方法**：1、减小Batch_Size和数据大小  2、查看在测试数据部分的代码中是否加上with torch.no_grad表示在测试数据时不进行反向传播和梯度计算 3、在训练数据代码部分前加上释放GPU数据的代码
## 问题三
**问题**：在命令行窗口下直接运行所要执行的代码即直接`python train.py`发生报错：ModuleNotFoundError，当让这种情况是在你train.py代码中还import了你自己写的其他py文件代码
**分析**：在运行Python的IDE中如果直接运行代码文件，OK没问题那是因为在软件IDE条件下，软件会帮你把你一个文件夹下其他的代码文件的sys.path设置好加入进去，但是如果在命令行下面运行就不一样没人帮你做这件事情，在IDE中import都是相对路径而不是绝对路径因而我们要做的就是在执行代码的头部加上一段代码使得其他文件能够加入sys的path中，根据import的机制，代码就会相对应找到相应的包（import机制详情请自行搜索查询）
**解决方法**：请在你要执行的代码最前面加上如下代码

```
import sys
import os
if __name__ == '__main__':
       sys.path.append(os.path.dirname(sys.path[0]))
```
## 问题四
**问题**：运行测试数据代码的时候报错，RuntimeError: Attempting to deserialize object on CUDA device 2 but torch.cuda.device_count() is 1
**分析**：大概就是说你训练的时候使用两个GPU训练但是现在你电脑只有一个GPU
**解决方法**：在torch.load的时候后面加上map_location='cuda:0'，比如
```
torch.load(args.load_model, map_location='cuda:0')
```
## 问题五
**问题**：具体的内容我给忘记了没有记下来，但是是在测试数据集代码运行时报错，大概内容是测试数据的张量大小是三维不符合模型中的四维数据模型
**分析**：在训练数据的时候数据都是以四维的形式即[batch, channel, H, W]输入进去，但是在测试数据的时候虽然输入的图像数据是三维的大小但是你需要同样的把它变换成四维数据
**解决方法**：给输入数据加上unsqueeze即`input_data = input_data.unsqueeze(0)`，0就是在第一维上添加一个1，输入就会变成[1, *, *, *]

