---
title: 深度学习：GPU云服务器的租用
date: 2021-05-01 17:57:39
img: /images/1.jpg
author: Yuri
top: false
summary: 租用GPU云服务器来进行深度学习训练
categories: 深度学习
tags: 
   - 深度学习
   - 云服务器
   - GPU
---
## 前言
本文是在笔者做深度学习相关研究的时候需要高算力GPU去运行代码因而选择去租借GPU云服务器，这里记下自己所接触的一些GPU服务器网站和使用技巧

## 一、推荐站点
1、[极链AI云](https://cloud.videojj.com/)，首当其冲的一定是这家，这家是我用过认为较为好用而且相对价格比较平均然后操作方便，还有最重要的一点就是它微信注册绑定送100元，如果是学生再送100元，真的很爽，而且这家的GPU种类比较多，从RTX2080Ti，Tesla P100，Tesla T4，Ampere A100等，还是比较多的种类的
2、[1024Lab云](https://www.1024gpu.top/home)，这家的话服务器是架设在国外的，租用界面使用Github上一个开源的GPU租用界面，这家的话是所有租用平台上最便宜的但是它的种类比较少，最好的就只有2080Ti，最差的就是1080，但是2080Ti显卡的价格是0.215美刀/小时，真的是最便宜的了，其他国内平台像RTX2080Ti都是需要￥3/h左右的，但是这家有个缺点就是它的交易是通过网上货币DBC进行支付而且这家的话注册送的DBC不多只有1000DBC算比较少，只能用个几小时
3、[矩池云GPU](https://www.matpool.com/host-market)，这家的话做的会比较大，和很多的高校和公司都有过合作所以它的价格必然会高一些，但是这家在开始的时候会送5块钱的券，而且这家的话优点在于它可以VNC远程访问图形化桌面，操作简单，而且他家的GPU种类是真的很多啊
4、[易学智能AI云](https://www.easyaiforum.cn/)这家的特点是贵，个人感觉很贵，然后它的优点就是真的真的很好操作而且很人性化等等，具体我就不介绍了
5、[MistGPU云](https://mistgpu.com/)，这家的特点就是便宜了，注册送8块钱券，便宜的话就是差不多一个tesla P100只要4块钱/小时，邀请他人使用注册的会再送8块钱，无上限

## 二、GPU云服务器使用
**1、访问工具**
访问GPU云服务器的方式无非两种，一种是命令行shell访问另一种就是图形化界面访问，但是大多好像都是只提供shell命令行访问即终端访问，可能是在云服务器端要每个用户都配备图形化界面消耗的空间和功率更大吧
终端访问工具：Xshell（这是最常用的终端访问），Jupyter（也挺好用的），Xftp（主机和云服务器间进行FTP传输文件的工具）
![Xshel界面](https://img-blog.csdnimg.cn/20201218222527832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20201218222626978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
图形化界面访问工具：Teamviewer（个人觉得挺好用的），向日葵，VNC，这些第三方软件都需要主机和云服务器都要安装相应软件才可以互相进行访问控制和文件传输
**2、使用方法**
这里将以极链云AI平台使用介绍，其实都大同小异的，主要访问工具是Xshell
（1）在极链云上选择服务器进行租用
![](https://img-blog.csdnimg.cn/20201218223208129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
（2）选择完成后系统产生GPU实例
![](https://img-blog.csdnimg.cn/202012182234070.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20201218223434761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
（3）打开Xshell软件进行配置连接
![](https://img-blog.csdnimg.cn/20201218223550980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
在 新建会话属性 窗口填写自定义名称，主机 填写从控制台复制出的登录指令域名部分，端口号 填写 -p 后的数字
![](https://img-blog.csdnimg.cn/20201218223610391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
点击左侧类别中的 用户身份认证，右侧 用户名 填写 root，密码填写从控制台复制出的密码。确认保存
![](https://img-blog.csdnimg.cn/2020121822364271.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
打开并连接
![](https://img-blog.csdnimg.cn/20201218223654658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20201218223718823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20201218223729310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
类似的Xftp也是相同的操作
**3、文件传输方法**
（1）使用各家平台的网盘进行传输，比如极链有提供自己的网盘，可以在启动服务器前先上传数据到网盘上，然后在机器对应的目录下去访问读取数据
![](https://img-blog.csdnimg.cn/2020121822421112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
（2）使用FTP传输文件，可以使用Xftp传输但是普遍速度很慢，也可以使用FileZilla，具体使用方法自行百度

## 参考文献
[1][极链AI云帮助文档](https://cloud.videojj.com/help/docs/data_manage.html#id4)

