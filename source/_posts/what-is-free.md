title:  关于free命令
date: 2016年11月24日22:44:33
categories:  "Linux"
tag: 
- bash
- linux
  comments: true
---

# 1. 主要回答以下问题：

* free是什么？
* free的使用方法？
* free命令显示的是什么？
* 通过free我们可以干什么？

# 2. 正文 

## 1. free是什么？

首先`man free`里说到的：

>  Display amount of free and used memory in the system
>
>  free  displays  the  total amount of free and used physical and swap memory in the system, as well as the buffers used by the kernel.  The shared memory column represents either the MemShared value (2.4 series kernels) or the Shmem value  (2.6  series  kernels  and  later)  taken  from  the /proc/meminfo file. The value is zero if none of the entries is exported by the kernel.

简要的说就是显示你系统的空闲内存和使用内存了。



输入`free -m`就是这样下面的东西了：
```
root@dev:~# free -m
             total       used       free     shared    buffers     cached
Mem:          7983       7627        356         33       1081       1589
-/+ buffers/cache:       4956       3026
Swap:            0          0          0
```


## 2. free的使用方法

通过`man free`我们可以看到主要有以下的用法：

* 设置显示的单位  
* ​