---
title: Git学习：记录整理Github项目时遇到的问题和方法
date: 2021-06-03 11:15:42
img: /images/5.png
author: Yuri
top: false
summary: 好久没有维护自己的Github了，本文记录了这些天整理个人Github仓库时候遇到的问题和学到的新东西，若有错误之处还望指出。
categories: Git
tags: 
   - Git
---
## 前言
好久没有维护自己的Github了，本文记录了这些天整理个人Github仓库时候遇到的问题和学到的新东西。

## 一、Github的Branch问题
github的Branch以前一直我记得都是以master作为default，不过不知道什么时候变成了main作为default，不过没什么太大影响，去查资料后发现master用作default还和一些历史因素相关，这里不再细讲，所以在push项目上去的时候要看好push到的是master还是main分支中去，个人比较喜欢default不是很喜欢改变。
## 二、Github上传项目失败
正常的通过Git上传项目的流程应该是这样
```
git init
git add .
git commit -m "介绍"
git branch -M main 或者 git branch -M master
git remote add origin GitHub项目地址
git push -u origin main 或者 git push -u origin master
```
不过后来我发现这样子上传项目的时候，第一在Github上创建一个空项目就行不要添加README，第二在本低要上传文件的根目录下写好README文件然后按上面的流程就行。
本人在创建仓库的时候随手也创建了README然后再按照上面步骤上传项目出现如下情况
```
error: failed to push some refs to ‘http://xxx/xxx.git’
hint: Updates were rejected because a pushed branch tip is behind its remote
hint: counterpart. Check out this branch and integrate the remote changes
hint: (e.g. ‘git pull …’) before pushing again.
hint: See the ‘Note about fast-forwards’ in ‘git push --help’ for details.
```
这样的原因是因为在GIthub仓库里面已经存在了README文件但是本地段却没有，所以这里建议在一开始创建项目的时候不要创建README

## 三、生成目录树结构
在Windows10下通过命令生成树形目录结构
**①下载tree插件**
在[tree插件](https://sourceforge.net/projects/gnuwin32/files/tree/1.5.2.2/)里面找到tree-1.5.2.2-bin.zip文件点击下载然后解压后将里面bin文件夹的tree.exe文件复制到你的Git的目录的/usr/bin/目录下即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210519181045537.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzExNjk3,size_16,color_FFFFFF,t_70)
**②在本低项目的目录下生成目录树**
使用下列指令生成
```
tree -I node_modules > tree.text
```
然后复制目录树文件下的内容到README文件中进行撰写即可

