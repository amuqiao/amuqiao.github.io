---
title: Pycharm配置autopep8
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-04 17:52:17
img:
password:
summary:
tags:
- Pycharm
- autopep8
categories:
- 工具
---

> Pycharm让代码自动转换成pip8风格

<!--more-->

Pycharm本身是有pep8风格检测的，当你敲得代码中不符合规范时，会有下划波浪线提示。为了去掉这些难看的波浪线，我们需要借助autopep8这个工具让代码自动转换成pip8风格。
#### 安装autopep8
```
pip install autopep8
```

#### 在Pycharm中配置autopep8
Preferences->Tools->External Tools->点击加号
![](http://phdc6jit1.bkt.clouddn.com/blog/2018-11-03-035621.png)
点击+号后出现配置选项
![](http://phdc6jit1.bkt.clouddn.com/blog/2018-11-03-032314.png)
```
- Name：Autopep8（可以随便取）
- Tools settings: 
    - Programs：autopep8 （前提是你已经安装了哦）
    - Parameters: --in-place --aggressive --aggressive $FilePath$`
    - Working directory: $ProjectFileDir$
- 点击Output Filters→添加，在对话框中的：Regular expression to match output中输入：$FILE_PATH$\:$LINE$\:$COLUMN$\:.*
```
配置完成External Tools出现aotopep8
![](http://phdc6jit1.bkt.clouddn.com/blog/2018-11-03-032417.png)

#### 使用
autopep8在pycharm中的使用：在Pycharm编辑其中新建一个python文件，编辑一些不符合pep8风格的代码；将鼠标放在该文件的编辑器中→右键→External Tools→点击Autopep8。这样你的代码就符合pep8的风格了。如下图所示
![](http://phdc6jit1.bkt.clouddn.com/blog/2018-11-03-040141.png)



### win10 Pyachrm 配置autopep8
File->Settings->Tools->External Tools-> 点击`+`添加

配置
```
Name: Autopep8（自定义名称）
Tool Settings:
    Program:C:\ProgramData\Anaconda4.3.0\Scripts\autopep8.exe
    Argumetns: --in-place --aggressive --aggressive $FilePath$
    Working direcroty: $ProjectFileDir$
    
Advanced Options:
    Output filters: $FILE_PATH$\:$LINE$\:$COLUMNS$\:.*
```

应用

`Tools`->`External Tools`-> `Autopep8`

参考：

* [Pycharm配置autopep8教程，让Python代码更符合pep8规范](https://segmentfault.com/a/1190000005816556)