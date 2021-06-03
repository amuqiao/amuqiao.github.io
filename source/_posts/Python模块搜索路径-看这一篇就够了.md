---
title: Python模块搜索路径(看这一篇就够了)
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-22 09:33:28
img:
password:
summary: 理解Python导包规则
tags:
- python
- 模块搜索路径
categories:
- 后端

---

> 由于某些原因，执行Python程序时，常常出现ImportError、ModuleNotFoundError等错误，归根究底，是当前需要的模块不在python搜索路径中。那么，Python 如何知道在哪里搜索模块的路径呢？

### 什么是模块搜索路径？
Python导入模块`hello`时，按照以下路径进行搜索
```
1.具有该名称的内置模块
2.在变量 sys.path 给出的目录列表中搜索名为 hello.py 的文件
```
`sys.path` 从这些位置初始化：
```
1.包含输入脚本的目录（或当前目录，当没有指定文件时）
2.PYTHONPATH（目录名列表，与 shell 变量 PATH 语法相同）
3.与安装相关的默认值
```
查看搜索路径
```
# '' 表示当前目录（当前脚本所在的路径）
$ python
>>> import sys
>>> sys.path
['', '/usr/local/var/pyenv/versions/3.6.5/lib/python36.zip', '/usr/local/var/pyenv/versions/3.6.5/lib/python3.6', '/usr/local/var/pyenv/versions/3.6.5/lib/python3.6/lib-dynload', '/Users/wangqiao/code/TH/THKarma/venv/lib/python3.6/site-packages']
```

### 添加模块搜索路径
为了解决上述问题，需要添加模块搜索路径，可以使用以下几种方式

#### 动态增加路径 
临时生效，对于不经常使用的模块，这通常是最好的方式，因为不必用所有次要模块的路径来污染 `PYTHONPATH`。

通过 `sys` 模块的 `append()` 方法在 Python 环境中增加搜索路径：
```
>>> import sys
>>> sys.path.append('/home/wang/workspace')
```

#### 修改 `PYTHONPATH` 变量 
永久生效，对于在许多程序中都使用的模块，可以采用这种方式。这将改变所有 Python 应用的搜索路径，因为启动 Python 时，它会读取这个变量，甚至不同版本的 Python 都会受影响。
```
vi ~/.bash_profile
export PYTHONPATH=$PYTHONPATH:/home/wang/workspace
或者
export PYTHONPATH=/Users/wangqiao/code/TH:$PYTHONPATH
source ~/.bash_profile  # 使配置生效
```

#### 增加 `.pth` 文件 
永久生效，这是最简单的、也是推荐的方式,
Python在遍历已知的库文件目录过程中，如果见到一个`.pth` 文件，就会将文件中所记录的路径加入到 `sys.path` 设置中，于是 `.pth` 文件说指明的库也就可以被 Python 运行环境找到了。
python中有一个`.pth`文件，该文件的用法是：

在 `/usr/local/lib/python3.5/site-packages` 或者当前虚拟环境下`site-packages` 中添加一个扩展名为 `.pth` 的配置文件（例如：my.pth），内容为要添加的路径：

```
/Users/wangqiao/code/TH/THKarma
/Users/wangqiao/code/TH
```
验证路径是否添加
```
$ python
>>> import sys
>>> sys.path
['', '/usr/local/var/pyenv/versions/3.6.5/lib/python36.zip', '/usr/local/var/pyenv/versions/3.6.5/lib/python3.6', '/usr/local/var/pyenv/versions/3.6.5/lib/python3.6/lib-dynload', '/Users/wangqiao/code/TH/THKarma/venv/lib/python3.6/site-packages', '/Users/wangqiao/code/TH/THKarma', '/Users/wangqiao/code/TH']
```
注意，如果添加了路径还是出现`ModuleNotFoundError`，将项目的上层路径也添加到搜索路径中


参考：
- [为Python添加默认模块搜索路径](https://www.jianshu.com/p/cb6447e1cf88)
- [Python 模块搜索路径](https://blog.csdn.net/liang19890820/article/details/76219560)
- [python在不同层级目录import模块的方法](https://www.cnblogs.com/luoye00/p/5223543.html)
- [python 导入模块的坑(帮助理解导包)](https://www.cnblogs.com/ydf0509/p/8298551.html)