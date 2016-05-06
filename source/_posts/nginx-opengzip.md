title: Nginx开启gzip
date: 2016-05-07 00:35:42
categories: "运维”
tag: 
- nginx
comments: true
---

nginx默认是开启gzip的

但默认只是对html开启gzip
需要做如下配置:
<!-- more --> 
```
gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript text/html;
gzip_types *;
gzip_http_version 1.0;
```

若使用反向代理，需要加上：
```
proxy_set_header Accept-Encoding "gzip";
```

测试是否打开Nginx：
```
curl -I -H "Accept-Encoding: gzip, deflate" "https://www.codemao.cn/components/underscore/underscore.js"
```