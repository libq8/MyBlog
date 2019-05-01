---
title: ' Hexo+github搭建个人博客网站（一）'
date: 2019-04-05 23:31:07
categories: 尝试集
tags: [ hexo , 个人博客]
description: 简单使用hexo搭建个人博客部署到github page
---



<!--more-->


>   环境说明：
>
>   	Windows10
>   	
>   	Node.js：
>   	
>   		 建立在Chrome上的JavaScript运行引擎
>   	
>   	Git： 
>   	
>   		一款免费、开源的分布式版本控制系统
>   	
>   	Github： 
>   	
>   		国内一款面向开发者的云端开发平台，提供代码托管，运行空间，质量控制，项目管理等功能
>   	
>   	Hexo： 
>   	
>   		快速、简洁且高效的博客框架
>   	
>   		1. 支持Markdown
>   	
>   		2. 轻量: 无需拥有后台及数据库
>   	
>   		3.一键部署: 可以通过Git或者ftp来将生成的静态页面部署到服务器或者主机空间中
>   	
>   		4. 插件和主题丰富: 满足各种个性化需求.



### 1. 准备工作

#### 1.1安装和配置Git

[Git官网](https://git-scm.com)

注意把Git加到环境变量中，方便使用。可以通过右键检查是否配置成功。



#### 1.2安装和配置Node.js

[Node.js下载地址](https://nodejs.org/en/)

打开cmd命令行输入指令检查版本：

>   node -v
>
>   npm -v



#### 1.3Github账户注册和配置

[Github官网](https://github.com/)

###### 1.3.1新建项目

注册成功后新建项目仓库create a new repository，注意项目命名格式必须为：账户名.github.io，通常勾选Public和initialize with README。

打开项目的setting，查看github page可以找到项目链接，打开链接（账户名.github.io）即可访问到项目，后续将使用hexo部署个人博客网站到该项目。

###### 1.3.2配置SSH key

创建仓库之后需要完成SSH key的配置以便更好地链接github服务器。打开cmd命令行输入指令检查配置

>    ssh -T git@github.com

###### 1.3.3域名绑定

绑定域名可以更加方便访问博客网站，如果是在服务器上部署绑定域名有利于使用更安全的HTTPS协议。

当然不绑定也是可以的，直接使用github page就好。具体绑定域名的流程请另查教程。



### 2.安装和使用Hexo

#### 2.1安装Hexo

1) 在合适的位置创建文件夹作为hexo部署网站的一个根目录，如：D:\blog；打开命令行进入该目录

2) 开始安装hexo，执行命令

> **npm install hexo -g**

3) 检查安装，执行命令

>   **hexo -v**

4) 初始化博客

>   **hexo init**

显示提示信息“INFO Start blogging with Hexo!"表示初始化成功

5) 安装软件依赖

>   **npm install**

6) 渲染并启动服务器

>   **hexo g** 	生成渲染source目录下的静态文件
>   **hexo s**   启动本地服务器

7)  打开浏览器访问 http://localhost:4000 查看效果

![博客网站初始效果](/image/hexo/003.PNG)



#### 2.2 绑定hexo和github page

1) 确定SSH key配置成功

2) 配置deployment

在博客根目录（本例为D:\blog）修改\_config.yml文件

~~~ 
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
	type: git
	repository: 
		github: git@github.com:username/username.github.io.git
~~~
repository值为步骤1.3.1中github项目的SSH

注意每一个键值对之间（即冒号:之后）必须有一个空格



#### 2.3新建博客

1) 进入博客的根目录（本例为D:\blog）执行新建博客命令：

>   **hexo new post "hello_world"**

在D:\source\\_posts文件目录下可以看到新建的hello_world.md文件

2）编辑hello_world.md文件

3）安装扩展：

>   **npm install hexo-deployer-git --save**

4）生成和部署博客：

>   **hexo clean**

>   **hexo d -g**

5）访问博客：$https://uername.github.io$





#### 参考教程：

[[使用Hexo+Github一步步搭建属于自己的博客（基础）](https://www.cnblogs.com/fengxiongZz/p/7707219.html)]

[[使用hexo+github搭建免费个人博客详细教程](https://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html)]
