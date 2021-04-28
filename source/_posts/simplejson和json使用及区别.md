---
title: simplejson和json使用及区别
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-22 09:17:39
img:
password:
summary:
tags:
- python
- json
- simplejson
categories:
- 后端
---


> json、simplejson效率比较：simplejson在效率上来得有优势，推荐用simplejson

<!--more-->

几个主要函数：dump、dumps、load、loads，带s跟不带s的区别： 带s的是对 字符串的处理，而不带 s的是对文件对像的处理。



### python json.dumps() json.dump()的区别

首先说明基本功能：

- dumps是将dict转化成str格式，loads是将str转化成dict格式。
- dump和load也是类似的功能，只是与文件操作结合起来了。

```
import simplejson as json

fw = open("test.txt","wb")
dict = {"name":xiaoming,"age":20,"school":"sysu"}
json.dump(dict,fw) # 存入文件
dict = json.load(open("test.txt","rb"))
for k,v in dict.items():
    print k,v
```

参考文档

- [python json.dumps() json.dump()的区别](http://www.cnblogs.com/wswang/p/5411826.html)