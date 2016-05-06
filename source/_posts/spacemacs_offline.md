title: Spacemacs 修改代理
date: 2016-05-07 00:28:45
categories: "开发"
tag: 
- emacs
- spacemacs
comments: true
---
## 1. 使用·env SHELL=/bin/bash emacs --insecure·进入

## 2. 修改init.el 

~/.emacs.d/init.el

```lisp
(setq url-http-proxy-basic-auth-storage
      (list (list "proxy.company:8080"
                  (cons "USERHERE"
                        (base64-encode-string "USERHERE:PASS")))))

```

<!-- more --> 

## 3. 修改.spaceemacs

```
dotspacemacs-elpa-https nil 
```
