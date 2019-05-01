---
title: 在阿里云ECSLinux系统上安装jupyter notebook #文章页面上的显示名称，一般是中文
permalink: 在阿里云ECSLinux系统上安装jupyter notebook
date: 2019-03-28 07:47:20 #文章生成时间，一般不改，当然也可以任意修改
categories: 软件安装 #分类
tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 在阿里云ECSLinux系统上安装jupyter notebook，使用公网IP访问
---

<!--more-->

## 1.安装anaconda

### 1.1 官网下载安装脚本文件

### 1.2 执行bash XXX.sh命令进行安装，最后是否添加到系统环境变量选项输入yes

### 1.3 重新登录或执行source ~/.bahrc命令加载环境变量

### 1.4 验证安装，执行conda -V命令查看conda版本信息

## 2. 安装ipython

### 2.1 打开ipython


```python
# ipython
#from notebook.auth import passwd
#passwd()
#此时会让你两次输入密码，然后就会生成秘钥,记得复制下来，配置密码的时候要用到
```

## 3.安装jupyter

### 3.1 执行安装命令


```python
# pip install jupyter
```

### 3.2 生成配置文件


```python
#jupyter notebook --generate-config
```

### 3.3 配置安装文件

> 
c.NotebookApp.ip='*'                 # 对外访问ip，*表示开放所有
c.NotebookApp.password = u'sha:ce...'     # 刚才根据密码生成的密钥
c.NotebookApp.open_browser = False       # 禁止自动打开浏览器
c.NotebookApp.port =8888              # 访问的端口（后期需要配置虚拟主机的安全组规则开放该端口）
c.NotebookApp.notebook_dir = u''         # 指定工作目录(即jupyter启动后默认的根目录）

## 4. 启动jupyter


```python
#jupyter notebook (root用户需加参数 --allow-root)
```

由于本例是在阿里云虚拟主机上操作，为了可以开放对外访问端口，需要新增安全组规则：

选择自定义TCP类型协议，8888/8888端口，授权对象为0.0.0.0/0

具体可参考：[阿里云 解决为什么不能使用公网IP地址访问部署的nginx项目](https://blog.csdn.net/LIU_KuLaLaLa/article/details/77772984?locationNum=6&fps=1)

浏览器访问https://ip:8888,注意一定要用http协议， 输入设置的密码登录

为了使得进程保持在后台运行，可以在jupyter ontebook后面加参数 &
