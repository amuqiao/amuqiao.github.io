---
title: Django ORM中原生JSONField的使用方法
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-21 14:28:53
img:
password:
summary: JSONField定义和查询示例
tags:
- django
- python
- orm
- jsonfield
categories:
- 后端

---



> Django最新版v3.1的主要更新之一便是完善了对JSON数据存储的支持，新增models.JSONField
> 和forms.JSONField，可在所有受支持的数据库后端上使用



目前支持的数据库以及对应版本主要有
- MariaDB 10.2.7+
- MySQL 5.7.8+
- Oracle
- PostgreSQL
- SQLite 3.9.0+

但个别Django的查询方法可能与部分数据库不兼容，例如contains
和contained_by
就不支持Oracle和SQLite数据库

### JSONField使用
```
from django.db import models
class Hero(models.Model):    
    name = models.CharField(max_length=200)    
    data = models.JSONField(null=True)  
    
    def __str__(self):        
        return self.name
```
通过models.JSONField

可指定此字段为存储类型为JSON格式。null=True

表示此字段可以为空，这个NULL指的是SQL NULL

如果想存储为JsonNULL，则可以使用Value('null')来实现


```
Hero.objects.create(name='coffee', data=Value('null'))
```

SQL NULL与JsonNULL的区别主要在is_null
的查询上不同，可以通过以下这个示例来理解下

```
>>> from django.db.models import Value
>>>>>> Hero.objects.create(name='ops')<Hero: ops>
>>> Hero.objects.create(name='coffee', data=Value('null'))<Hero: coffee>
>>>>>> Hero.objects.filter(data=None)<QuerySet [<Hero: coffee>]
>>>> Hero.objects.filter(data=Value('null'))<QuerySet [<Hero: coffee>]
>>>>>>> Hero.objects.get(name='ops').data>>> Hero.objects.get(name='coffee').data
>>>>>> Hero.objects.filter(data__isnull=True)<QuerySet [<Hero: ops>]
>>>> Hero.objects.filter(data__isnull=False)<QuerySet [<Hero: coffee>]>
```

### JSONField查询
```
data = {'age': 12,'group': 
                {
                    'name': 'ow1',
                    'skill': [
                            {'name': 'swim', 'rank': 'A+'},
                            {'name': 'shot', 'rank': None}
                            ]
                    
                }
    
        }
Hero.objects.create(name='ops-coffee.cn', data=data)

Hero.objects.create(name='ops-coffee', data={'age':16})

```
当想要查询age为12的数据时可以这样查询
```
Hero.objects.filter(data__age=12)
```

当想要查询group的name为ow1的数据时可以这样查询
```
Hero.objects.filter(data__group__name='ow1')
```
当想要查询group下skill中第一个数据的name值为swim的数据时可以这样查询
```
Hero.objects.filter(data__group__skill__0__name='swim')
```

当想要查找包含group键的所有数据时，可以通过has_key来实现

```
Hero.objects.filter(data__has_key='group')

```
当想要查找同时包含group
键和age
键的所有数据时，可以通过has_keys
来实现
```
Hero.objects.filter(data__has_keys=['group','age'])

```

当想要查找包含group
键或者age
键的所有数据时，可以通过has_any_keys
来实现
```
Hero.objects.filter(data__has_any_keys=['group','age'])

```
当想一次性查找包含age
为12
且group
的name
为ow1
的数据时，可以通过contains
来实现
```
Hero.objects.filter(data__contains={'age':12,'group': {'name': 'ow1'}})

```
JSONField除了支持以上查询方式外，对于ORM所提供的大部分其他查询方式同样支持，例如
- icontains
- endswith
- iendswith
- iexact
- regex
- iregex
- startswith
- istartswith
- lt
- lte
- gt
- gte

使用起来也是非常方便
```
Hero.objects.filter(data__age__lte=12)
Hero.objects.filter(data__group__name__startswith='ow')

```


参考文档

- [Django ORM中原生JSONField的使用方法](https://www.modb.pro/db/28826)