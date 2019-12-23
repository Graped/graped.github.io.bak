---
title: python的安装和pycharm的使用
date: 2018-11-19 11:36:58
tags: python
categories: python
---
# 环境安装
## 在windows 10上安装ptyhon 2.7.13和3.7.0
- 去网站 [www.python.org/downloads](https://www.python.org/downloads//) 
- 根据所需下载相对应版本，然后双击安装，一直保持默认即可。

备注: 在安装时候可以通过勾选添加系统环境变量来自动添加系统环境变量，如下图
![image](https://i.imgur.com/8fUy8LC.png)

安装好了输入python3和python会显示如图效果：
![image](https://s1.ax1x.com/2018/11/19/FSI0PA.png)


## 在Centos7.3上安装python2和python3
Centos7 默认自带python2.7
```python
[root@GrapedLinux ~]# python
Python 2.7.5 (default, Nov  6 2016, 00:28:07) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-11)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
centos7.3安装python3.7.0 请跳转：[centos安装python3](https://www.cnblogs.com/anxminise/p/9650206.html)

全部安装好后，在centos上也可以同时使用pyhton和python3
![image](https://s1.ax1x.com/2018/11/19/FSoGJs.png)

## 在Window 10上安装安装 PyCharm 
- 去网站 [www.jetbrains.com/pycharm](https://www.jetbrains.com/pycharm) 下载最新的pro版本pycharm
- 然后根据网上的教程进行激活操作，点击[跳转参考资料](https://mp.weixin.qq.com/s?__biz=MzI0OTc0MzAwNA==&mid=100000449&idx=1&sn=5ed69988cfe8cd32ad3c2f3e30e88314&chksm=698d91325efa182499a2a2c11aea26b01e115764b2c347315fc07b0efc8350260d36a61ea432&mpshare=1&scene=1&srcid=1119hpaT8zTLV0hYXAcyvvG0#rd)

## 设置PyCharm 抬头 
第一次使用的时候，我们可以通过File->setting->Editot->Code Style->File and Code Templates ->Python Script 来设置成如下的抬头
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : ${DATE} ${TIME}
# @Author  : Graped
```

## 第一个python例子
```
# -*- coding:utf-8 -*-
# @Time    : 2017/4/8 17:40
# @Author  : Graped
name = raw_input("print input your name: ")  # raw_input 此函数用来和用户进行参数交互
print("hello " + name)
```

## Pycharm常用快捷键
- Ctrl + c
- Ctrl + x
- Ctrl + v
- Ctrl + d
- Ctrl + shif + n
- Ctrl + shif + f
- Ctrl + 鼠标左键
- Ctrl + alt + 方向左/右键    注意和系统屏幕设置的快捷键冲突
- Ctrl + a；  ctrl + alt + l
- Alt + enter
- Ctrl + /
- Tab       shift +tab


## 如何去运行一个python程序

1. linux系统
```bash
chmod + x test.py
./test.py
python test.py
```

2. windows系统

cmd中： 
```
python test.py
```

3. pycharm  

直接点击运行 

Pycharm的调试模式:

- 断点：  就是程序执行到这个地方停下来
- F7： Step Into 相当于eclipse的f5就是 进入到代码
- F8：Step Over 相当于eclipse的f6 跳到下一步
- F9： resume programe 恢复程序或者执行到下一个断点