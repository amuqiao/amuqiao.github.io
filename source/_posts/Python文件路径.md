---
title: Python文件路径
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-02 14:41:34
img:
password:
summary:
tags:
- python
- 文件路径
categories:
- 后端

---


```
# -*- coding: utf-8 -*-
# @Time    :2018/11/19 1:16 PM
# @Author  : wangqiao

# -*- coding: UTF-8 -*-
import os.path
import sys


def print_path():

    print("项目绝对路径 os.path.dirname(os.path.abspath(__file__)):{}".format(os.path.dirname(os.path.abspath(__file__))))

    print('在原路径上添加路径:', os.path.join('/home/python', 'Desktop'))
    print('文件绝对路径:', os.path.abspath(sys.argv[0]))
    print('文件当前目录', os.path.dirname(__file__))
    print(
        '文件上级目录',
        os.path.abspath(
            os.path.join(
                os.path.dirname(__file__),
                '..')))
    print(
        '同级其他目录',
        os.path.abspath(
            os.path.join(
                os.path.dirname(__file__),
                '../database')))
    print(
        '文件上上级目录',
        os.path.abspath(
            os.path.join(
                os.path.dirname(__file__),
                '../..')))
    print(
        '文件上上上级目录',
        os.path.abspath(
            os.path.join(
                os.path.dirname(__file__),
                '../../..')))

    print('csv', os.path.join(os.path.dirname(__file__), 'csv/app_store'))
    print(
        'csv',
        os.path.abspath(
            os.path.join(
                os.path.dirname(__file__),
                'csv/app_store')))


if __name__ == '__main__':
    print_path()
    print(os.path.abspath(__file__))
    print(os.path.commonpath(os.path.abspath(__file__)))
    print(os.path.dirname(os.path.abspath(__file__)).split('/')[-1])
```