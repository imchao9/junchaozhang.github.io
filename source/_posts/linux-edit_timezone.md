title: Linux修改时区
date: 2016-05-07 00:50:24
categories: "运维"
tag: 
- linux
comments: true
---

tzselect 

tzselect：
执行tzselect命令-->选择Asia-->选择China-->选择east China - Beijing, Guangdong, Shanghai, etc-->然后输入1。过程如下图：

cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
