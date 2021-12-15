---
title: Linux下Clash配置教程
date: 2021-05-15 01:15:19
img: /images/6.png
author: Yuri
top: false
summary: 本文是在Ubuntu16.04系统amd64架构上进行实验，同时也可以适用于其他的系统或架构，若有错误之处还望指出。可以给我留言
categories: Clash
tags: 
   - Linux
   - Clash
---
## 前言
本文是在Ubuntu16.04系统amd64架构上进行实验，同时也可以适用于其他的系统或架构

## 一、安装Clash
在Clash的GitHub[下载地址](https://github.com/Dreamacro/clash/releases)上安装相对应版本和系统架构的Clash

## 二、配置相关文件
**①配置config.yml文件**
在自己的机场里找到clash的订阅地址，然后复制到网址栏里面即可下载yml文件，然后复制到存放clash的目录之下
**②配置Country.mmdb文件**
该文件是全球IP的库，即需要这个库才可以访问国外下载地址可以访问GEOIP官网[mmdb文件](https://dev.maxmind.com/geoip/geoip2/geoip2-city-country-csv-databases/)
当然也可以从这里现成的下载[Country.mmdb](https://asset.10101.io/externalLinksController/chain/Country.mmdb?ckey=8hY08nReECvarf4iCbIHXFlx80mL43thUQmjf6KnKh512UdFMDeEn%2bH5I%2bkJOnl/)

## 三、运行Clash
在clash的目录下的终端输入以下命令

```
chmod +x clash文件
```

```
./clash -d .
```

## 四、配置网络
在设置——网络——手动里面修改网络配置如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210515011352304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)


