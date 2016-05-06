title: Nwjs 制作Mac客户端
date: 2016-05-07 00:30:38
categories: "开发"
tag: 
- nwjs
- mac
comments: true
---

##1. 将app.nw放到Contents/Resources里

https://github.com/nwjs/nw.js/wiki/How-to-package-and-distribute-your-apps

##2. 替换nodewebkit图标

Contents/Resources/nw.icns: icon of your app.
Contents/Info.plist: the apple package description file.
<!-- more --> 
###1. 使用IconKit制作图标

###2. 更换图标
显示简介-直接拖到图标处即可

##3. 打包为dmg
http://bbs.feng.com/read-htm-tid-6724285.html