---
title: python中requests使用指南
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-23 09:52:54
img:
password:
summary:
tags:
- python
- requests
categories:
- 后端

---
> python模块requests使用指南

<!--more-->

- 使用get方法带参数请求时，是params=参数字典
- 使用post方法带参数请求时，是post=参数字典
- 在url编码时，有些编码是把空格编码为+，有些则是编码为%20
- requests会对参数进行自动编码

### get方法带参数请求
```
params={
    'cid':567464,
    'page':1,
    'key':'',
    'language':1,
    'gtk':6,
    '_cid':567464,
    '_mid':34949,
    '_dt':'2019-05-03 13:03:08',
    '_sign':'e74c8c52618a64a454dd7f12aff3cc1c'
    }
def getFun(url,data):
    ret=requests.get(url,params=params)
    print(ret.url)
    return ret
```

### post方法带参数请求
```
data={
    'cid':567464,
    'page':1,
    'key':'',
    'language':1,
    'gtk':6,
    '_cid':567464,
    '_mid':34949,
    '_dt':'2019-05-03 13:03:08',
    '_sign':'e74c8c52618a64a454dd7f12aff3cc1c'
    }
def getFun(url,data):
    ret=requests.post(url,data=data)
    print(ret.url)
    return ret
```