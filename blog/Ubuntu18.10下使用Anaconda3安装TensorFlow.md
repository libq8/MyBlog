---
title:  Ubuntu18.10下使用Anaconda3安装TensorFlow #文章页面上的显示名称，一般是中文
date: 2019-03-28 07:47:20 #文章生成时间，一般不改，当然也可以任意修改
categories: 软件安装 #分类
tags: [Ubuntu, anaconda, TensorFlow] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: Ubuntu18.10下使用Anaconda3安装TensorFlow记录

---

<!--more-->

# 1.环境说明（tensorflow只支持64位）
> #### Ubuntu18.10-64位
> #### Anaconda3-1.7.2
> #### conda-4.6.2
> #### python-3.7.1
> ![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-ffab241321570bbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 2.新建虚拟环境
~~~
conda create -n env_name python=version
#env_name为新建的环境名，version为已安装的Python版本号
~~~
本例使用Python3.7.1创建虚拟环境tensorflow_3.7.1
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-5370f817ca4f4838.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
创建成功，提示激活和退出指令
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-8786c5e7f7787f11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 3.查看已有虚拟环境
~~~
conda env list
~~~
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-0550f1b7e17d7dfa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
base为默认虚拟环境，tensorflow3.7.1为刚刚新建的虚拟环境

# 4.激活虚拟环境
~~~
conda activate env_name
~~~
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-8c90b085112fd482.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 5.安装TensorFlow
~~~
pip install tensorflow
~~~
![image](http://upload-images.jianshu.io/upload_images/7659992-91e1d379caadb8ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 6.测试
打开Python3，输入以下测试代码
~~~
import tensorflow as tf
 
message = tf.constant('hello world')
sess=tf.Session()
 
print(sess.run(message))
~~~
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-d1a02afdc9af6813?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
执行结果：
>#### b'hello world'

# 7.退出虚拟环境
~~~
conda deactivate
~~~
![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/7659992-1fbcb0624beeb171.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 8.参考
[ubuntu18.04 anaconda安装tensorflow](https://blog.csdn.net/weixin_42001089/article/details/81807197)
[Ubuntu18.04基于anaconda安装tensorflow](https://blog.csdn.net/qq_30975647/article/details/80097184)