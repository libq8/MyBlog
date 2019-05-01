---
title:  Ubuntu18.10安装Anaconda3 #文章页面上的显示名称，一般是中文
date: 2019-03-28 07:47:20 #文章生成时间，一般不改，当然也可以任意修改
categories: 软件安装 #分类
tags: [Ubuntu, anaconda] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: Ubuntu18.10安装Anaconda3记录
---

<!--more-->

# 1.下载安装包
#### 根据Ubuntu已安装的Python版本选择对应的安装文件
## [官网](https://www.anaconda.com/distribution/#macos)
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-74d15e550e4da428?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## [清华镜像网站（下载速度更快）](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-7bf4d21cb14432a8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 2.安装
#### 拷贝文件到Ubuntu的downloads目录，打开终端进入目录cd /downloads
### 2.1执行命令sudo bash Anaconda3-5.3.1-Linux-x86_64.sh
![image](http://upload-images.jianshu.io/upload_images/7659992-9ec3009cb5180dd0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2.2按Enter键阅读注册信息，输入“yes”同意注册条款
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-0f1a89cd37038791.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2.3选择安装位置
![image](http://upload-images.jianshu.io/upload_images/7659992-d6b7b2b72f315383.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2.4添加环境变量
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-e577055a77fb838a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2.5安装完成
![image](http://upload-images.jianshu.io/upload_images/7659992-eca1ad69b66041b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2.6根据个人选择安装VScode
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-c7e955a206b7d16b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 3.验证安装
> ### anaconda -V
> ### conda -V
> ### ipython notebook
> ![image](http://upload-images.jianshu.io/upload_images/7659992-02259b5a5a3d17f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> ![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-9483543fc222aaf6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 4.Anaconda 更新与卸载
## 4.1更新Anaconda
>  #### conda update conda
>  #### conda update anaconda
## 4.2卸载Anaconda
> #### sudo rm -rf /usr/local/anaconda3
> #### 删除上面~/.bashrc和/etc/profile的修改
> #### 清空隐藏文件： rm -rf ~/.condarc ~/.conda ~/.continuum

# 5.参考
[Linux(Ubuntu 18.04)上安装Anaconda步骤详解](https://www.jb51.net/article/149932.htm)