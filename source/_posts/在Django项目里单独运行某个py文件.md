---
title: 在Django项目里单独运行某个py文件
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-30 17:44:39
img:
password:
summary:
tags:
- django
- python

categories:
- 后端

---

> 在Django项目里单独运行某个py文件

<!--more-->

```python
import os, sys, django

# BASE_DIR 寻找项目根目录

BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
sys.path.append(BASE_DIR)

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'project_name.settings')
django.setup()

from app.models import Person

if __name__ == "__main__":
    all =Person.objects.all().values()
    print(all)
```