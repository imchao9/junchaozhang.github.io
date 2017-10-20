title:  Node.js console.log引发的思考
date: 2017年10月07日22:54:35
categories:  "node.js"
tag: 
- node.js
- comments: true
---

# 1. 背景

最近突然思考了一个问题，在一个for循环内：

```javascript
for(;;) {
  let a = "test";
  console.log(a);
}
```

```javascript
let a = '';
for(;;) {
  a = "test";
  console.log(a);
}
```

这两者会不会有什么差异

一个疑问

我声明的一个变量究竟放到哪里了



https://stackoverflow.com/questions/33283446/loop-of-console-log-in-nodejs

https://github.com/nodejs/node/issues/11568

https://github.com/nodejs/node/issues/4280

