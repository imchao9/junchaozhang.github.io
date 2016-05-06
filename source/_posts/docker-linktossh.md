title: 教你SSH到docker容器里
date: 2016-01-27 21:46:18
updated: 2016-04-29 01:09:36
categories: “运维”
tag: 
- docker
- ssh
comments: true
---

## 1. 查看容器Ip

``` 
docker inspect --format='{{.NetworkSettings.IPAddress}}' $CONTAINER_ID
```

## 2. 添加网络规则
让docker容器内的22端口映射到宿主机上的12345

``` 
iptables -t nat -A DOCKER -p tcp -m tcp  --dport 12345 -j DNAT --to-destination 192.168.1.1:22
```
<!-- more --> 
## 3. 修改docker容器的密码
```
passwd 
```

## 4. 为docker容器安装ssh服务
```
sudo apt-get install openssh-server
修改`/etc/ssh/sshd_config`的：
	PermitRootLogin yes
```

## 5. 测试一下
```
ssh -p 20072 root@xxx.com 
``` 

## 6. 打开Webstorm
在Tools-Deployment中添加一个server

result:
![WebStorm ssh server][image-1]

[image-1]:	http://i.imgur.com/8V0QkP4.png?1