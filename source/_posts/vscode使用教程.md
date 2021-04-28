---
title: vscode使用教程
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-28 08:56:39
img:
password:
summary:
tags:
- vscode
- python
- ide

categories:
- 开发工具

---


### python

#### 在vscode中调试django
1.创建/打开django项目

2.在调试侧边栏里，选择创建launch.json

![创建launch.json](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/VbnGJr.png)
![选择python环境](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/kzbo9I.png)
![选择django项目](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/htbiNC.png)
![launch.json](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/Z8iTuK.png)

配置参考
```
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Django",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/manage.py",
            "args": [
                "runserver",
                "0.0.0.0:8001"
            ],
            "django": true
        }
    ]
}
```