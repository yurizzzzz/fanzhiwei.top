---
title: Windows10下完全移除Ubuntu系统并重装
date: 2021-10-25 12:32:40
img: /images/13.png
author: Yuri
top: false
summary: 笔记本电脑安装了Windows10和Ubuntu双系统，需要完全移除Ubuntu系统并重装新的Ubuntu系统，若有错误之处还望指出。可以给我留言
categories: Linux
tags: 
   - Linux
---

# 前言
笔记本电脑安装了Windows10和Ubuntu双系统，需要完全移除Ubuntu系统并重装新的Ubuntu系统

# 一、删除Ubuntu占用的系统空间

- 这里我的Ubuntu装在了磁盘1下的空间中，进入磁盘管理界面直接对Ubuntu占用的磁盘空间进行删除，如图所示:  
![](https://z3.ax1x.com/2021/10/25/5hGzeP.png)  
- 如果不清楚磁盘分区情况可先```Win+R```进入CMD然后输入```disk part```查看即可知道如下图显示未知的就是我分配给Ubuntu系统的所有空间.  
![](https://z3.ax1x.com/2021/10/25/5hJXhF.jpg)

# 二、删除EFI中的Ubuntu启动引导项

- Win+R进入CMD命令行并输入diskpart进入磁盘管理
- list disk显示磁盘
- select disk 0选择Windows10所安装的磁盘
- list partition显示磁盘下的分区
- select partition 1选择Windows10所在的那个系统分区差不多是100M那个
- assign letter = P
- 查看我的电脑发现多出P盘
- 使用管理员权限打开记事本
- 使用记事本打开P盘中的EFI文件夹并直接删除里面的ubuntu文件夹
- 回到diskpart命令行，输入remove letter = P

# 三、删除Ubuntu的启动引导项

- 重启机子进入BIOS
- 选择Boot选项找到启动项中的ubuntu并直接按照提示进行删除即可

# 四、安装最新的Ubuntu系统

- 准备U盘把Ubuntu系统镜像写入U盘，使用[Rufus](https://rufus.ie/zh/)进行镜像写入
- 进入磁盘管理给笔记本电脑分配空闲的磁盘空间给Ubuntu
- 插入U盘重启电脑进入BIOS设置U盘为启动项
- 开始安装
- 设置必要的几个系统分区

