---
title: Python列表项为dict的去重
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-21 11:15:27
img:
password:
summary:
tags:
- python
- dict
categories:
- 后端

---


有如下列表

```
li = [{'a': 1}, {'b': 2}, {'a': 1}]
```

如果采用set的去重方式，则会报错


```
li = list(set(li))
>>>TypeError: unhashable type: 'dict'
```

可以用下面的方法：
第一种：reduce


```
def deleteDuplicate(li):
    func = lambda x, y: x if y in x else x + [y]
    li = reduce(func, [[], ] + li)
    return li

>>> deleteDuplicate(li)
>>>[{'a': 1}, {'b': 2}]
```

第二种：


```
def deleteDuplicate(li):
    temp_list = list(set([str(i) for i in li]))
    li=[eval(i) for i in temp_list]
    return li

>>> deleteDuplicate(li)
>>>[{'a': 1}, {'b': 2}]
```

第三种：


```
>>>li = [dict(t) for t in set([tuple(d.items()) for d in li])]
>>>li
>>>[{'a': 1}, {'b': 2}]
```

建议使用第三种，因为速度更加快，经过测试相同的数据，第三种方法速度比第一种的10倍。