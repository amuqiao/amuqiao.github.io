---
title: Pycharm配置autopep8
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-28 09:41:03
img:
password:
summary:
tags:
- pycharm
- python
- autopep8
- ide

categories:
- 开发工具

---



> Pycharm让代码自动转换成pip8风格

<!--more-->

Pycharm本身是有pep8风格检测的，当你敲得代码中不符合规范时，会有下划波浪线提示。为了去掉这些难看的波浪线，我们需要借助autopep8这个工具让代码自动转换成pip8风格。
### 安装autopep8
```
pip install autopep8
```

### mac 配置
Preferences->Tools->External Tools->点击加号
![AcfE3V](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/AcfE3V.png)
点击+号后出现配置选项
![z0xfgQ](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/z0xfgQ.png)
添加autopep8配置
![AmDFFI](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/AmDFFI.png)
```
- Name：Autopep8（可以随便取）
- Tools settings: 
    - Programs：autopep8 （前提是你已经安装了哦）
    - Parameters: --in-place --aggressive --aggressive $FilePath$`
    - Working directory: $ProjectFileDir$
- 点击Output Filters→添加，在对话框中的：Regular expression to match output中输入：$FILE_PATH$\:$LINE$\:$COLUMN$\:.*
```
配置完成External Tools出现aotopep8

使用

autopep8在pycharm中的使用：在Pycharm编辑其中新建一个python文件，编辑一些不符合pep8风格的代码；将鼠标放在该文件的编辑器中→右键→External Tools→点击Autopep8。这样你的代码就符合pep8的风格了。


### windows 配置
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