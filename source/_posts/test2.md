title:  记一次及其负载过高解决过程
date: 2016年11月27日23:30:21
categories:  "Linux"
tag: 
- linux
  comments: true
---

# 1. 背景

服务器持续的发生负载超过0.8，甚至在半夜都会发生，于是查看监控，发现

![ 平均负载](http://ww2.sinaimg.cn/large/006y8lVagw1fa73xycoggj31jy0kwdkq.jpg)

其中这一台服务器的负载特别高。

# 2. 过程

首先我们要知道负载是什么

负载：load average



参考：

http://www.wikiwand.com/en/Load_(computing)

http://www.brendangregg.com/blog/2017-08-08/linux-load-averages.html

负载

进程

线程

