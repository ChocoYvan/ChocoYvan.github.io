---
layout:     post
title:      Python环境下Scrapy爬虫框架安装
subtitle:   Anaconda环境下安装Scrapy
date:       2017-11-06
author:     巧不巧克力/ChocoYvan
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - Python
    - 爬虫
---

##  配置Python环境
首先确认电脑配置好python环境，如果是mac用户，进入终端输入以下指令，如果是windows，进入cmd输入以下指令
`python --version`
![](http://upload-images.jianshu.io/upload_images/1591780-8d71c532912883bb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如未安装，百度搜索相应教程安装python，2、3版本都可以的，mac会自带python2。相应python安装过程可以参考[静觅博客](http://cuiqingcai.com/912.html)
## 安装Scrapy
安装好python环境后输入
`pip install scrapy`
即会自动搜索安装好scrapy模块。

这里我是用mac和anaconda环境，作为python的一个集成环境anaconda相当好用，如果有学习机器学习的意向，推荐使用anaconda。安装时选择添加到环境变量path，则无需再配置环境变量，非常方便，下载来就可以用了。
[anaconda在这里下载](https://www.anaconda.com/download/)
windows用户安装完成后，打开 Anaconda Prompt ，输入
`conda list`
查看是否安装有scrapy，一般不会自带scrapy，需要自己安装。输入
`conda install scrapy`
则会自动搜索安装scrapy。
mac用户打开终端查看python版本是否为anaconda。是则安装scrapy，不是再输入以下指令
`export PATH=~/anaconda/bin:$PATH`
![](http://upload-images.jianshu.io/upload_images/1591780-2f6c451a126aa680.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
之后重复上面的步骤，用pip install或conda install指令安装scrapy即可。 


