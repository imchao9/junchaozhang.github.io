title: 在使用Travis-CI过程中遇到的一些问题
date: 2016-05-07 00:46:44
categories: "运维”
tag: 
- travis-ci
comments: true
---

##问题：

File decryption fails (wrong final block length) on Windows

```
bad decrypt
139709116544672:error:0606506D:digital envelope routines:EVP_DecryptFinal_ex:wrong final block length:evp_enc.c:532:
The command "openssl aes-256-cbc -K encrypted048ea30036f2key−ivencrypted048ea30036f2key−ivencrypted_048ea30036f2_iv -in file.enc -out file -d" failed and exited with 1 during .
Your build has been stopped.
```
<!-- more --> 

##答案：
There is a report of this function not working on a local Windows machine. Please use a Linux or OS X machine.
windows生成的ssh-key长度和linux/mac不一样，key不一样就无法将私钥解开

##相关：
https://github.com/travis-ci/travis-ci/issues/4746
https://docs.travis-ci.com/user/encrypting-files/

##总结：
在这个过程中，我体会到了若理解了原理，解决起问题来会事半功倍，若我一开始弄清楚了openssl的错误就会知道是什么问题。