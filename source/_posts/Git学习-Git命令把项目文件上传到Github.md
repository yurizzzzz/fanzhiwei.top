---
title: 'Git学习:Git命令把项目文件上传到Github'
date: 2021-05-01 21:06:58
img: /images/7.JPG
author: Yuri
top: false
summary: Git上传文件和代码
categories: Git
tags: 
   - Git
---
## 前言
记录使用Git命令上传工程项目到个人Github上，怕自己以后会忘记，操作系统是在Windows上进行，学会Windows操作，Linux上自然会，都差不多。

**一、下载Git**
百度搜索Git，到官网上下载Git对应版本型号
**二、在本地的Github个人中心创建仓库**
![](https://img-blog.csdnimg.cn/20200721230532710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**三、配置SSH**
1、在需要上传的工程文件夹下面右键点击Git bash here输入命令

```
ssh-keygen -t rsa -C  "邮箱" 
```
2、进入SSH密钥存放地址我的电脑——C盘——用户——.ssh/文件夹，打开id_ras.pub文件，如果打不开就用记事本打开，然后复制里面的所有内容
3、点击New SSH Key然后创建好后将复制的内容粘贴到Github的SSH密钥栏处
![](https://img-blog.csdnimg.cn/20200721231037327.png)
![](https://img-blog.csdnimg.cn/20200721231129993.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
4、检查密钥是否配置成功，在刚刚的Git bash here的命令行下面输入

```
ssh -T git@github.com
```
**四、上传工程文件**
这些操作都是在要上传的工程文件夹下面，就在工程文件的根目录下就行
1、配置账号名和邮箱

```
git config --global user.email "邮箱"
git config --global user.name ‘名字’
```
2、初始化Git空间

```
git init
```
3、上传文件到暂存区（如果上传工程目录下所有文件就是add .如果上传某个文件就是add xxxx.xx）

```
git add .
```
4、暂存区的文件上传到Git仓库空间

```
git commit -m "此处输入项目注释语句"
```
5、建立Github仓库和工程文件夹下Git的联系（xxxxxxxx是指Github建立仓库的SSH地址）
![](https://img-blog.csdnimg.cn/20200721231902883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)


```
git remote add origin xxxxxxxxxxxx
```
6、克隆Readme文档到本低目录下

```
git pull --rebase origin master
```
7、上传文件到Github上

```
git push -u origin master
```
**五、参考文献**
![](https://img-blog.csdnimg.cn/20200930163208745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70#pic_center)

[使用Git上传工程项目](https://www.cnblogs.com/Amywj/p/13219108.html)


