---
title: Ubuntu16.04下Clash配置出现的问题和记录
date: 2021-05-01 21:04:48
img: /images/6.JPG
author: Yuri
top: false
summary: Ubuntu下Clash的配置
categories: Linux
tags: 
   - Ubuntu
   - Clash
---
## 前言
本文是关于在Linux下Clash时候出现的一些问题，写这篇博客仅此记录
**一、更改了连接密码后怎么办**
如果在网址端修改了连接密码，那么相应的UUID号也会跟着改变，所以相应的配置文件也同样需要改变，Linux下配置文件就是config.yaml这是最主要的文件;在改变连接密码后，把原来文件夹下的config.yaml文件删除掉，然后先运行一下程序后再关闭则会出现一个不完整的config文件然后关闭，再输入下面的代码，然后刷新文件夹即可生成一个新的config文件，然后再和原来一样运行回去就可以解决

```
sudo curl 网址 >> config.yaml
```

**二、参考文献**
[https://blog.csdn.net/qq_27036771/article/details/106748409?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase](https://blog.csdn.net/qq_27036771/article/details/106748409?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)
