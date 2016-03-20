##1. 添加网络规则
让docker容器内的22端口映射到宿主机上的12345
```
iptables -t nat -A DOCKER -p tcp -m tcp  --dport 12345 -j DNAT --to-destination 192.168.1.1:22
```

##2. 修改docker容器的密码
```
passwd 
```

##2. 为docker容器安装ssh服务
修改`/etc/ssh/sshd_config`的
```
PermitRootLogin yes
```

##3. 测试一下 
```
ssh -p 20072 root@xxx.com 
```

##4. 打开Webstorm
在Tools-Deployment中添加一个server

result:
​

