title: Nwjs 制作windows客户端
date: 2016-05-07 00:28:45
categories: "开发"
tag: 
- nwjs
- windows
comments: true
---

## 1. 更换nodewebkit图标
http://keenwon.com/1311.html

### 1, 制作图标
使用Axialis IconWorkshop

### 2. 更换图标
使用Resource Hacker

## 2. 使用nodewebkit将应用打包成exe
http://segmentfault.com/a/1190000002453237

打包：在windows下打包test文件夹下文件为test.zip打包，并修改名字为test.nw
```
打包exe: copy /b nw.exe+test.nw test.exe
```

<!-- more --> 

## 2. 使用Inno Setup打包为setup.exe

http://blog.csdn.net/hougelou/article/details/www.c

创建快捷方式：
http://yedward.net/?id=104
```
[Tasks]
Name: "desktopicon"; Description: "{cm:CreateDesktopIcon}"; GroupDescription: "{cm:AdditionalIcons}"; Flags: checkablealone
Name: "quicklaunchicon"; Description: "{cm:CreateQuickLaunchIcon}"; GroupDescription: "{cm:AdditionalIcons}"; Flags: checkablealone
```

新建文件夹：
```
[Setup]
AppendDefaultDirName=no
```
