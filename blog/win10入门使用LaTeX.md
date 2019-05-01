---
title: win10入门使用LaTeX #文章页面上的显示名称，一般是中文
date: 2019-03-28 07:47:20 #文章生成时间，一般不改，当然也可以任意修改
categories: 软件安装 #分类
tags: [LaTex] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: windows10安装使用LaTeX（TeXlive+TeXstuido）记录
---

<!--more-->
 > ### 环境的配置——TeXlive+TeXstuido
# 1.下载安装TeXlive
### 1.1下载安装包和工具
从官方站点下载TeX Live 安装包。
下载地址：[官网](http://mirror.ctan.org/systems/texlive/Images/texlive2018.iso)，[清华大学镜像站](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive2018.iso)，[中国科技大学镜像站](https://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2018.iso)

安装需要使用虚拟光驱软件，推荐一个绿色工具即[UltraISOv9.6.2.3059](http://www.latexstudio.net/wp-content/uploads/2014/11/UltraISOv9.6.2.3059.zip)

### 1.2开始安装
解压打开UltraISOv9.6.2.3059
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-d9d01c6e8ac2ef2f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
加载下载好的ISO文件
![image](http://upload-images.jianshu.io/upload_images/7659992-aaf37d613d99632d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
右键打开
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-2bc1de90bc006572.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
右键以管理员身份运行install-tl-advanced
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-49007724f75b30a5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
安装开始
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-41aab6952c7a6145?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
安装结束
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-bb294f2e1e37567f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 2.安装TeXstudio
### 2.1下载安装文件
## [官网地址](https://sourceforge.net/projects/texstudio/)
打开安装文件，确定安装路径后开始安装。
### 2.2配置
Options > Configure TeXstudio> General > Language改成zh_CN（中文化）

Options > Build（构建）> Default Bibliography Tool（默认文献工具）设置成BibTeX（此处是管理参考文献的相关设置，下文有说明）

Options > Build（构建）> Default Compiler（默认编辑器）设置成XeLaTeX（为了实现对中文的支持）

# 3.简单使用
### 3.1中英混排
使用UTF8编码并导入ctex宏包可以完成中文文档的编辑。
测试代码：
~~~
\documentclass[UTF8]{ctexart}
\title{你好，world!}
\author{Liam}
\date{\today}
\begin{document}
\maketitle
\section{你好中国}
中国在 East Asia.
\subsection{Hello Beijing}
北京是 capital of China.
\subsubsection{Hello Dongcheng District}
\paragraph{Tian'anmen Square}
is in the center of Beijing
\subparagraph{Chairman Mao}
is in the center of 天安门广场。
\subsection{Hello 北京}
\paragraph{北京} is an international city。
\end{document}
~~~

### 3.2数学公式
##### 3.2.1简写形式：
LaTeX中的数学模式有两种形式:inline 和 display。前者是指在正文插入行间数学公式,后者独立排列,可以有或没有编号。
行间公式(inline):用\$...\$将公式括起来。
块间公式(displayed)，用\$\$...\$\$将公式括起来是无编号的形式，块间元素默认是居中显示的。

##### 3.2.2标准单个公式环境
~~~
\usepackage{amsmath, amsfonts, amssymb} % math equations, symbols（导入宏包）
\begin{equation}
...
\end{equation}
~~~
它是最一般的公式环境，表示一个公式，默认情况下之表示一个单行的公式，但是它的功能可以通过内嵌各种其他环境进行扩展。

##### 3.2.3align（多个公式）
这是最基本的对齐环境，其他多公式环境都不同程度地依赖它。
采用“&”分割各个对齐单元，使用“\\”换行，每行是一个公式，都会独立编号。
~~~
\begin{equation}
	\begin{aligned} \label{eq:rasl}
	J^{(D)}( θ^{( G)}， θ^{( D)}) = -\frac{1}{2}E_{x\sim p_{r}(x)}[\log{D(x)}]
	\\
	-\frac{1}{2}E_{z\sim p_{g}(z)}[\log{(1-D(G(z)))}]
	\end{aligned}
	\end{equation}
~~~
此处参考[Latex常见公式环境与对齐方式小节](https://blog.csdn.net/yanxiangtianji/article/details/17583723)

### 3.3插入图片
##### 3.3.1直接插图
~~~
\includegraphics[scale=0.2]{f1.png}
~~~
##### 3.3.2使用浮动体
~~~
\usepackage{graphicx}   % import figures

\begin{figure}[ht]
\centering
\includegraphics[scale=0.2]{f1.png}
\caption{生成式对抗网络结构图}
\label{fig:label}
\end{figure}
~~~
此处参考[Latex中插图总结（一）](https://blog.csdn.net/chichoxian/article/details/52588833)

### 3.4参考文献
~~~
\documentclass{ctexart}

\bibliographystyle{plain}   %引用的样式%
\begin{document}
    这是一个参考文献引用：\cite{name1} %大括号内为相应文献的引用标签
    \bibliography{text}     %导入参考文献库文件%
\end{document}
~~~
##### 3.4.1建立bib文件
打开谷歌搜索（或镜像）网站，输入论文名称，点击引用，选择BibTeX
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-284e079ced67932a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
拷贝内容到text.bib文件
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-ffed57f35d37ef85?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 3.4.2在正文中引用
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-7b13f7cc4a45a012?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
效果
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-8f9f1bce5eb03b8f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 4.模板使用
### 1. [LaTeXtemplates.com](http://www.latextemplates.com/)网站是非常不错的模板分享网站，收集了包括书信，报告，论文，演示文稿，简历等等模板，整体收集模板质量很不错。
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-3e3ef4820b46e24a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2. [GitHub上](https://github.com/MartinThoma/LaTeX-examples)有作者收集的非常好的模板收集，也收集了大量的tikz等等例子，可下载，选择自己喜欢的模板使用。
![image](http://upload-images.jianshu.io/upload_images/7659992-1b6495d16b26c2ab?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 3. [overleaf.com](https://www.overleaf.com/)也可以找到很多期刊的模板代码，还支持在线编辑


### 4. [LaTeX开源小屋](http://www.latexstudio.net/archives/category/latex-templates/thesis-template)里也有很多论文模板资源。
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-47184a220fc7e682?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 5.模板的简单使用
选择需要的模板下载并解压，在TeXstudio中打开tex文件，点击构建并查看即可在右边看到模板效果。
使用模板进行报告编辑时直接在左边源码进行修改。
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-38b32d9793fb9c3f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 5.参考
[LaTeX零基础入门教程](https://www.jianshu.com/p/3e842d67ada2)
[Latex中插图总结（一）](https://blog.csdn.net/chichoxian/article/details/52588833)
[Latex常见公式环境与对齐方式小节](https://blog.csdn.net/yanxiangtianji/article/details/17583723)