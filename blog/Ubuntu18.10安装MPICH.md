---
title:  Ubuntu18.10安装MPICH #文章页面上的显示名称，一般是中文
date: 2019-03-28 07:47:20 #文章生成时间，一般不改，当然也可以任意修改
categories: 软件安装 #分类
tags: [Ubuntu, MPICH] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: Ubuntu18.10安装MPICH记录


---

<!--more-->

## 1.官网下载压缩文件：[官网下载网址](http://www.mpich.org/downloads/)
本例选择mpich-3.3
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-b4df10f4a9225c9f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.编译器检查
> ### gcc --version 
> ### g++ --version 
> ### gfortran --version 
> ### echo $SHELL
> ![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-24a4820e068d4ac7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 如果尚未安装对应的编译器，可执行sudo apt install XXX命令进行安装


## 3.解压与配置
创建安装文件夹 :
> ##### mkdir mpich-install
> 解压文件：
> ##### 执行命令tar -zxvf mpich-3.3.tar.gz或右键提取
> 进入解压文件夹：
> ##### cd mpich-3.3
> 配置安装路径：
>
> ##### ./configure -prefix=/home/[username]/software/mpich-install

## 4.编译安装
> ##### make
> ##### make install

## 5.添加环境变量
> ##### sudo gedit ~/.bashrc
> 末尾添加
> ##### export MPI_ROOT=/home/[username]/software/mpich-install（必须是绝对路径）
> ##### export PATH=\$MPI_ROOT/bin:$PATH
> ##### export MANPATH=\$MPI_ROOT/man:$MANPATH
> 激活环境变量
>
> ##### source .bashrc 


## 6.测试
查看命令
> ##### which mpicc
> 使用官方测试文件：mpirun或mpiexec命令
> ##### mpirun -np 10 ./examples/cpi 
> ##### mpiexec -n 4 ./examples/cpi
> ![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-4f600482a53d05e2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> 样例测试：保存mpi_test.cpp文件，使用mpic++编译后使用mpirun执行
```C++
#include <mpi.h>
#include <iostream>

using namespace std;

int main (int argc, char **argv)
{
	int myid, numprocs;
	MPI_Init (&argc, &argv);
	MPI_Comm_rank (MPI_COMM_WORLD, &myid);
	MPI_Comm_size (MPI_COMM_WORLD, &numprocs);
	cout<<"Hello from process_"<<myid<<endl;
	MPI_Finalize();
	return 0;
}
```
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-80ba8bbc662d3464.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参考：
[ubuntu16.04一步一步安装配置mpich](https://blog.csdn.net/u010177634/article/details/53048371)
[我的并行计算之路（一）Ubuntu 16.04下的MPI安装](https://blog.csdn.net/qq_30239975/article/details/77703321)