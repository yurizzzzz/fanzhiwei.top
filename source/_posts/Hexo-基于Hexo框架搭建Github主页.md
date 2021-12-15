---
title: 'Hexo:基于Hexo框架搭建Github主页'
date: 2021-05-04 18:09:54
img: /images/7.png
author: Yuri
top: false
summary: 本文主要是在GitHub's Page的基础上使用Hexo开源框架对Page进行优化，并且使用自定义域名来访问GitHub's Page，所以此个人主页的搭建是在自己的Github账号上，服务器自然也是在Github那边，只不过我们要做的就是对原来的Github's Page进行优化而已。
categories: Hexo
tags: 
   - Hexo
---
## 前言
本文主要是在GitHub's Page的基础上使用Hexo开源框架对Page进行优化，并且使用自定义域名来访问GitHub's Page，所以此个人主页的搭建是在自己的Github账号上，服务器自然也是在Github那边，只不过我们要做的就是对原来的Github's Page进行优化而已。

## 一、安装Git
自行在[Git官网](https://git-scm.com/)下载相对应版本的Git

## 二、安装Node.js
**①下载安装**
Hexo开源项目是基于Node.js，所以，首先需要电脑上安装Node.js和配置系统路径。
[Node.js下载地址](https://nodejs.org/en/) 
关于Node.js的路径配置可能会有点小坑，可能同学们在网上找的Node.js的配置教程都是比较早的，所以很可能已经过时，笔者在后续安装中出现的问题归根结底主要由于Node.js的系统路径配置，这里给出我参考的认为能较好解决配置问题的一篇博文。
[Node.js的安装与配置](https://blog.csdn.net/qq_40593308/article/details/110559838?spm=1001.2014.3001.5506)
**②验证安装的成功性**
```
node -v
```
```
npm -v
```
若各出现版本号即表示安装成功

## 三、安装npm的国内镜像cnpm
由于npm命令较为常用并用来下载Hexo所需的一些插件但是国内直接使用npmmingling下载速度较慢，所以这里提供国内淘宝云镜像，下次要用npm命令的时候就可以用cnpm代替。

**①下载cnpm淘宝云镜像**

```
npm config set registry https://registry.npm.taobao.org
```
**②验证cnpm安装的成功性**

```
npm config get registry
```
若返回https://registry.npm.taobao.org则表示安装成功

## 四、安装Hexo框架
**①使用cnpm下载Hexo**

```
cnpm install -g hexo-cli
```
**②验证Hexo框架安装成功性**

```
hexo -v
```

## **五、初始化个人主页项目**
**①在本地电脑端找个位置新建一个文件夹用来存放项目文件**
**②进入新建的文件夹然后右键点击`git bash here`**
**③输入以下命令来初始化**
```
// 初始化项目
hexo init
```

```
// 运行主页，可以提前在http://localhost:4000上预览主页效果
hexo s
```

## 六、Github创建个人主页项目
**①点击新建一个repositories，切记项目命名为自己的username.github.io**
**②在上面新建的文件夹的目录下面点击`git bash here`进入命令行**
**③安装部署工具**
```
cnpm install --save hexo-deployer-git
```
**④修改blog下面的config.yml文件**

找到config.yml最下面的deploy部分，改为如下代码，注意修改自己的username使此项目对应到自己的GitHub项目上去
```
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master
```
**⑤部署上传Github**

修改优化自己的主页后通过以下命令来控制GitHub's Page的生成和修改
```
hexo clean
```
```
hexo g
```
```
hexo d
```
## 七、自定义域名绑定
**①在阿里云（其他平台也可以）上购买域名**
**②买完域名后需要解析到自己的Github项目中**

```
ping username.github.io
```
得到项目的IP地址后，进入阿里云解析自己的域名到一个IPV4地址，解析记录设置两个分别用www和@开头作为解析前缀，TTL生效时间设置为10

**③新增CNAME文件**

在原来创建的文件夹即/hexo/source/创建一个CNAME文件（无后缀名文件），在文件里面写下在阿里云购买的域名。

## 参考
[1] [Hexo+Github从0搭建个人博客](http://weimumu.top/2020/ckjco6flf00022gnl4c163vst/)
[2] [最快速搭建个人博客](https://rika0-0.github.io/2020/04/08/%E6%9C%80%E5%BF%AB%E9%80%9F%E5%9C%B0%E6%90%AD%E5%BB%BA%E7%82%AB%E4%B8%BD%E7%9A%84%E4%B8%AA%E4%BA%BA%E7%BD%91%E7%AB%99or%E5%8D%9A%E5%AE%A2/)
[3] [Hexo官网](https://hexo.io/zh-cn/)

