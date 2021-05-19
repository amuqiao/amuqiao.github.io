---
title: django中使用celery
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-18 09:42:53
img:
password:
summary:
tags:
- django
- django-celery-beat
- celery
categories:
- 后端

---

> celery很容易集成到Django框架中，实现定时任务需要安装django-celery-beat插件.

### 前言
#### 添加定时任务
通过django-celery-beat 在后台添加定时任务
- 安装django-celery-beat
- 在tasks.py中添加定任务
- 在setting.py中注册定时任务
- 启动celery beat和worker
- django-celery-beat会自动发现定时任务
- 在CELERY_RESULT_BACKEND中查看定时任务的执行结果

#### 在ORM中查看任务执行结果
通过django-celery-results 在后台查看定时任务的执行结果
- 安装 django-celery-results
- 登录django admin后台，在CELERY RESULTS中查看任务执行结果


### 项目配置
新建立django项目django_init，tasks文件，用于定义任务：
```python
django_init
├── app01
│  ├── __init__.py
│  ├── apps.py
│  ├── migrations
│  │  └── __init__.py
│  ├── models.py
│  ├── tasks.py
│  └── views.py
├── manage.py
├── django_init
│  ├── __init__.py
│  ├── settings.py
│  ├── urls.py
│  └── wsgi.py
└── templates
```
在项目目录`django_init/django_init/`目录下新建`celery.py`:
```python
# -*- coding: utf-8 -*-
import os
from celery import Celery
# 设置django环境
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'django_init.settings')

app = Celery('django_init')

# 使用CELERY作为前缀，在settings中写配置
app.config_from_object('django.conf:settings', namespace='CELERY')

# 自动发现任务,发现任务文件每个app下的task.py
app.autodiscover_tasks()
```

`django_init/django_init/__init__.py`中添加配置：
```python
from .celery import app as celery_app

__all__ = ['celery_app']
```


### 定义定时任务
任务定义在每个tasks文件中，`djcelery_beat_app/tasks.py`：

创建`djcelery_beat_app`应用
```python
python manage.py startapp djcelery_beat_app
cd djcelery_beat_app
touch tasks.py
```

在`djcelery_beat_app/tasks.py`中添加定时任务
```python
# -*- coding: utf-8 -*-
import time
from django_init import celery_app


@celery_app.task
def print_date():
    """定时打印时间"""
    timestamp = time.time()
    # 将时间戳转换为本地时间字符串
    time_str = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(timestamp))
    print(f"django_celery_beat 定时任务: 打印当前时间{time_str}")
```

### 注册定时任务
在setting.py 中添加
```
CELERY_IMPORTS = (
    'django_celery_beat.tasks'
)
```

### 在视图中触发定时任务
```
from django.http import JsonResponse
from app01 import tasks

# Create your views here.

def index(request,*args,**kwargs):
    res=tasks.add.delay(1,3)
    #任务逻辑
    return JsonResponse({'status':'successful','task_id':res.task_id})
```



### 在Django后台中配置定时任务
在django后台中配置定时任务功能同样是靠beat完成任务发送功能，需要安装django-celery-beat插件。以下将介绍使用过程。

#### 安装django-celery-beat
```
pip install django_celery_beat
```
#### 注册APP

```
INSTALLED_APPS = [
    ....   
    'django_celery_beat',
]
```

#### 数据库变更
```
python3 manage.py migrate django_celery_beat
```

#### 分别启动woker和beat
```
#启动beat 调度器使用数据库
celery -A django_init beat -l info --scheduler django_celery_beat.schedulers:DatabaseScheduler  

#启动woker
celery -A django_init worker -l info
```

#### 配置admin 
`/django_init/django_init/urls.py`
```
from django.urls import path, include
from django.contrib import admin
 
urlpatterns = [
    path('admin/', admin.site.urls),
]
```
#### 创建用户
```
python3 manage.py createsuperuser 
```

#### 启动Django服务
```python
python manage.py runserver 127.0.0.1:8001
```

#### 添加定时任务
打开浏览器访问[http://127.0.0.1:8001/admin/](http://127.0.0.1:8001/admin/)
![PERIODIC TASKS](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/E94vxx.png)

- Clocked
- Crontabs 计划任务定义
- Intervals 间隔任务
- Periodic tasks 任务定义
- Solar events 根据太阳升起、降落定义任务


Periodic tasks 任务定义
![oAsOHd](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/oAsOHd.png)
![xkOgGk](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/xkOgGk.png)

保存后，可在celery workers中看到定时任务被执行的日志

### 动态添加定时任务
```
>>> from django_celery_beat.models import PeriodicTask, IntervalSchedule

# 添加定时间隔周期
#IntervalSchedule.DAYS
#IntervalSchedule.HOURS
#IntervalSchedule.MINUTES
#IntervalSchedule.SECONDS
#IntervalSchedule.MICROSECONDS
>>> schedule, created = IntervalSchedule.objects.get_or_create(
...     every=10,
...     period=IntervalSchedule.SECONDS,
... )
#添加定时任务
>>> PeriodicTask.objects.create(
...     interval=schedule,                  # we created this above.
...     name='Importing contacts',          # simply describes this periodic task.
...     task='proj.tasks.add',  # name of task.
... )
#下面是添加带参数的定时任务
>>> import json
>>> from datetime import datetime, timedelta

>>> PeriodicTask.objects.create(
...     interval=schedule,                  # we created this above.
...     name='Importing contacts',          # simply describes this periodic task.
...     task='proj.tasks.add',  # name of task.
...     args=json.dumps(['arg1', 'arg2']),
...     kwargs=json.dumps({
...        'be_careful': True,
...     }),
...     expires=datetime.utcnow() + timedelta(seconds=30)
... )

```

### Django的orm作为结果存储
除了redis、rabbitmq能做结果存储外，还可以使用Django的orm作为结果存储，当然需要安装依赖插件，这样的好处在于我们可以直接通过django的数据查看到任务状态，同时为可以制定更多的操作，下面介绍如何使用orm作为结果存储。

安装


```
pip install django-celery-results
```

配置settings.py，注册app

```
INSTALLED_APPS = (
    ...,
    'django_celery_results',
)
```
修改backend配置，将redis改为django-db

```
#CELERY_RESULT_BACKEND = 'redis://10.1.210.69:6379/0' # BACKEND配置，这里使用redis

CELERY_RESULT_BACKEND = 'django-db'  #使用django orm 作为结果存储
```
修改数据库


```
python manage.py migrate django_celery_results
```
此时会看到数据库会多创建
![vYNA6P](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/vYNA6P.png)

在 /django_init/venv/lib/python3.7/site-packages/django_celery_results/models.py 可以看到
TaskResult表的定义
```python
class TaskResult(models.Model):
    """Task result/status."""

    task_id = models.CharField(
        max_length=getattr(
            settings,
            'DJANGO_CELERY_RESULTS_TASK_ID_MAX_LENGTH',
            255
        ),
        unique=True, db_index=True,
        verbose_name=_('Task ID'),
        help_text=_('Celery ID for the Task that was run'))
    task_name = models.CharField(
        null=True, max_length=255, db_index=True,
        verbose_name=_('Task Name'),
        help_text=_('Name of the Task which was run'))
    task_args = models.TextField(
        null=True,
        verbose_name=_('Task Positional Arguments'),
        help_text=_('JSON representation of the positional arguments '
                    'used with the task'))
    task_kwargs = models.TextField(
        null=True,
        verbose_name=_('Task Named Arguments'),
        help_text=_('JSON representation of the named arguments '
                    'used with the task'))
    status = models.CharField(
        max_length=50, default=states.PENDING, db_index=True,
        choices=TASK_STATE_CHOICES,
        verbose_name=_('Task State'),
        help_text=_('Current state of the task being run'))
    worker = models.CharField(
        max_length=100, db_index=True, default=None, null=True,
        verbose_name=_('Worker'), help_text=_('Worker that executes the task')
    )
    content_type = models.CharField(
        max_length=128,
        verbose_name=_('Result Content Type'),
        help_text=_('Content type of the result data'))
    content_encoding = models.CharField(
        max_length=64,
        verbose_name=_('Result Encoding'),
        help_text=_('The encoding used to save the task result data'))
    result = models.TextField(
        null=True, default=None, editable=False,
        verbose_name=_('Result Data'),
        help_text=_('The data returned by the task.  '
                    'Use content_encoding and content_type fields to read.'))
    date_created = models.DateTimeField(
        auto_now_add=True, db_index=True,
        verbose_name=_('Created DateTime'),
        help_text=_('Datetime field when the task result was created in UTC'))
    date_done = models.DateTimeField(
        auto_now=True, db_index=True,
        verbose_name=_('Completed DateTime'),
        help_text=_('Datetime field when the task was completed in UTC'))
    traceback = models.TextField(
        blank=True, null=True,
        verbose_name=_('Traceback'),
        help_text=_('Text of the traceback if the task generated one'))
    meta = models.TextField(
        null=True, default=None, editable=False,
        verbose_name=_('Task Meta Information'),
        help_text=_('JSON meta information about the task, '
                    'such as information on child tasks'))

    objects = managers.TaskResultManager()

    class Meta:
        """Table information."""

        ordering = ['-date_done']

        verbose_name = _('task result')
        verbose_name_plural = _('task results')

    def as_dict(self):
        return {
            'task_id': self.task_id,
            'task_name': self.task_name,
            'task_args': self.task_args,
            'task_kwargs': self.task_kwargs,
            'status': self.status,
            'result': self.result,
            'date_done': self.date_done,
            'traceback': self.traceback,
            'meta': self.meta,
            'worker': self.worker
        }

    def __str__(self):
        return '<Task: {0.task_id} ({0.status})>'.format(self)

```

在admin后台查看task result的执行结果
![mQlnbp](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/mQlnbp.png)


### Celery参考配置
```python
# Celery 配置

CELERY_BROKER_URL = 'redis://127.0.0.1:6379/0'  # Broker配置，使用Redis作为消息中间件

# CELERY_RESULT_BACKEND = 'redis://10.1.210.69:6379/0'  # BACKEND配置，这里使用redis
CELERY_RESULT_BACKEND = 'django-db'  # 使用django orm 作为结果存储

CELERY_RESULT_SERIALIZER = 'json'  # 结果序列化方案
# 注册定时任务
CELERY_IMPORTS = (
    'djcelery_beat_app.tasks'
)
```

### 参考文档
- [Django中使用Celery](https://www.cnblogs.com/wdliu/p/9530219.html)
- [分布式任务队列Celery入门与进阶](https://www.cnblogs.com/wdliu/p/9517535.html)