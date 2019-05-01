---
title: 使用VMware14安装Ubuntu18.10 #文章页面上的显示名称，一般是中文
permalink: 使用VMware14安装Ubuntu18.10
date: 2019-03-28 07:47:20 #文章生成时间，一般不改，当然也可以任意修改
categories: 软件安装 #分类
tags: [VMware, ubuntu] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 使用VMware14安装Ubuntu18.10记录
---

<!--more-->


# 使用VMware14安装Ubuntu18.10

## 1.新建虚拟机

选择推荐的典型配置---> 稍后安装操作系统---> 选择Linux系统的Ubuntu版本---> 虚拟机命名与安装位置---> 指定磁盘容量为20G，选择存储为单个文件---> 完成

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-a7cbab7c7011fe30?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-3402873523135db9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-f6ceb934f56491e7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-0907ed8a3f4d5f90?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 2.下载安装系统

### 2.1下载镜像文件

下载网址：官网(https://www.ubuntu.com/download/desktop  , http://www.ubuntu.org.cn/download)或阿里巴巴镜像网站（<https://opsx.alibaba.com/mirror>）

选择对应版本和位数的镜像文件，本例使用阿里镜像网站（推荐使用，下载较快）下载64位的Ubuntu18.10

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-eae4660fc9fe0452?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![1549254212445](http://upload-images.jianshu.io/upload_images/7659992-dcf12c0ebe12d2fc?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.2使用已下载的镜像文件

打开VMware14，编辑虚拟机设置---> CD硬件选择ISO映像，指定刚刚下载的映像文件---> 开启虚拟机，选择语言按提示输入用户信息完成安装

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-c9dcbdd43b5ceb25?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-af0c5e0355e64fba?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-a3b832e8e98ab4c3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 3.安装VMware Tools

安装VMware tools有两种方法，一是在完成系统安装后按提示解压文件完成安装，二是独立安装最新的open VMware tools。推荐使用第二种方法，主要适用于Ubuntu16和Ubuntu18，有的按第一种安装成功后会推荐更新为第二种，以下主要介绍第二种。

自主安装open VMware tools只需打开terminal执行三条指令：

#### 1.更新

>   ### sudo apt-get upgrade

#### 2.安装open-tools-desktop

>   ### sudo apt-get install open-vm-tools-desktop -y

#### 3.重启生效（无需重启也可以使用）

>   ### sudo reboot



## 4.安装完成
