---
title: win10入门使用Github #文章页面上的显示名称，一般是中文
permalink: win10入门使用Github
date: 2019-03-28 07:47:20 #文章生成时间，一般不改，当然也可以任意修改
categories: 软件安装 #分类
tags: [git，github] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: windows10安装使用github记录

---

<!--more-->

# 1.注册GitHub官网账号

### [GitHub官网地址](https://github.com)

# 2.下载和安装Git
### [git下载网址](https://git-scm.com/download)
运行安装文件，根据个人选择配置路径，其他选项选取默认项

# 3.获取个人密匙
打开Git Bash，输入以下指令获取SSH KEY
~~~
$ ssh-keygen -t rsa -C "your_email@youremail.com"
#your_email@youremail.com改为你在github上注册的邮箱，路径与密码选择回车默认即可
~~~
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-92612db7f09bfe95?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
本例在/c/Users/libq8生成.ssh文件夹，进入.ssh文件夹打开id_rsa.pub文件，复制里面的key。

# 4.登录GitHub添加密匙
登录个人GitHub账号，选择settings ->SSH and GPG keys，点击new SSH key，命名title并粘贴生成的SSH key。
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-c95c03187ce9e9df?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 5.绑定连接GitHub
~~~
$ ssh -T git@github.com
~~~
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-40155e475518fea3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 6.设置username和email
~~~
$ git config --global user.name "your name"
$ git config --global user.email "your_email@youremail.com"
~~~

# 7.克隆仓库到本地
### 7.1新建仓库：
登录GitHub账号，选择your repositories->new
![image](http://upload-images.jianshu.io/upload_images/7659992-5a7ff616a3f35306.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-525fea46becb905a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 7.2查看仓库网址：
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-4bc6f920a1c03b82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 7.3输入克隆指令
~~~
git clone git@github.com:libq8/mygit.git
~~~
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-3c14538acb8ddee8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 8.在本地文件夹添加文件并上传
#### 8.1进入本地文件夹，创建test.txt文件，在Git Bash中使用ls指令查看
#### 8.2提出更改（添加到暂存区）
~~~
git add <filename>
git add *
~~~

#### 8.3提交到HEAD
~~~
git commit -m "代码提交信息"
~~~
#### 8.4提交到远端仓库
~~~
git push origin master
~~~
分支是用来将特性开发绝缘开来的。在创建仓库时，master 是"默认的"分支。
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-0b60f7da61faca44?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 8.5查看远程仓库
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-1f6ea23f9875441c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 9.参考
[GitHub 新手详细教程](https://blog.csdn.net/Hanani_Jia/article/details/77950594)
[Github 简明教程](http://www.runoob.com/w3cnote/git-guide.html)