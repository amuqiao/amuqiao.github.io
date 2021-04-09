---
title: Python日期转换
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-09 15:41:12
img:
password:
summary:
tags:
- Python
- datetime
- time
- 时间戳
categories:
- 后端
---


> Python日期转换、datetime模块和time模块的使用

<!--more-->
### Datetime 模块
> datetime是Python处理日期和时间的标准库。

#### 获取utc时间
```python
import datetime

utc_now = datetime.datetime.utcnow()
utc_now_date = datetime.datetime.utcnow().date()  # 获取年月日
utc_now_utctimetuple = datetime.datetime.utcnow().utctimetuple()  # 获取utc时间戳

> 2021-04-09 08:03:23.513941
> 2021-04-09
> time.struct_time(tm_year=2021, tm_mon=4, tm_mday=9, tm_hour=8, tm_min=3, tm_sec=23, tm_wday=4, tm_yday=99, tm_isdst=0)
```

#### 将datetime格式转换成字符串
```python
import datetime

# datetime.datetime(year, month, day, hour,minute, second）microsecond)
d0 = datetime.datetime(2021, 4, 3, 17, 10, 20)
d0_str = d0.strftime('%Y-%m-%d %H:%M:%S')

> 2021-04-03 17:10:20
```

#### 将字符串转换为datetime格式
```python
import datetime

d1 = datetime.datetime.strptime('2021-4-3 17:10:20', '%Y-%m-%d %H:%M:%S')

> 2021-04-03 17:10:20
```

#### 求时间差
```python
import datetime

d1 = datetime.datetime(2017, 5, 8)
d0 = datetime.datetime(2017, 4, 29)   
day_difference = (d1-d0).days # 4月份共30天 相差9天(不包括5月8日)
second_difference = (d1-d0).total_seconds()  # 计算相差秒数

> 9
> 777600.0
```

#### 日期加减
```python
import datetime

# timedelta 入参可选(days: float = ..., seconds: float = ..., microseconds: float = ...,milliseconds: float = ..., minutes: float = ..., hours: float = ...,weeks: float = ...)

d0 = datetime.datetime(2021, 4, 3, 17, 10, 20)
next_day = d0 + datetime.timedelta(days=2)

> 2021-04-05 17:10:20
```

#### 转换为unix时间戳
```python
import time
import datetime

d0 = datetime.datetime(2021, 4, 3, 17, 10, 20)
ans_time = time.mktime(d0.timetuple()) 

> 1617441020.0
```
#### 将unix时间戳转换为datetime
```python
import datetime

dtime = datetime.datetime.fromtimestamp(1617441020.0) 

> 2021-04-03 17:10:20
```

#### 输出带有utc时区的datime类型
```
import pytz
from datetime import datetime
utc = pytz.utc


def now() -> datetime:
    """
    返回带有utc时区的aware time
    """
    return datetime.utcnow().replace(tzinfo=utc)


if __name__ == '__main__':
    print(now())
    print(now().strftime('%Y-%m-%d %H:%M:%S'))

# 输出
2019-04-28 04:20:13.259841+00:00
2019-04-28 04:20:13
```

### time 模块
> time.time() 返回当前时间的时间戳（1970纪元后经过的浮点秒数，UTC标准时间戳）

```
import time

ans_time = time.time()

> 1617957802.867904
```

#### 将时间戳转换成时间字符串
```python
import time

timestamp = 1617441020.0
# 将时间戳转换为本地时间字符串
time_str = time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(timestamp)) 
> 2021-04-03 17:10:20

# 将时间戳转换为utc时间字符串
time_str = time.strftime('%Y-%m-%d %H:%M:%S',time.gmtime(timestamp)) 
> 2021-04-03 09:10:20

# 将当前时间戳转换本地时间为字符串
time_str = time.strftime('%Y-%m-%d %H:%M:%S')  
> 2021-04-09 16:49:32

# 将当前时间戳转换本地时间为字符串 同上
time_str = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(time.time()))  
> 2021-04-09 16:49:32

# 将当前时间戳转换为UTC时间字符串
time_str = time.strftime('%Y-%m-%d %H:%M:%S', time.gmtime(time.time()))  
> 2021-04-09 08:49:32
```

### 日期格式转换
python中时间日期格式化符号
```
%y 两位数的年份表示（00-99）
%Y 四位数的年份表示（000-9999）
%m 月份（01-12）
%d 月内中的一天（0-31）
%H 24小时制小时数（0-23）
%I 12小时制小时数（01-12）
%M 分钟数（00-59）
%S 秒（00-59）
%a 本地简化星期名称
%A 本地完整星期名称
%b 本地简化的月份名称
%B 本地完整的月份名称
%c 本地相应的日期表示和时间表示
%j 年内的一天（001-366）
%p 本地A.M.或P.M.的等价符
%U 一年中的星期数（00-53）星期天为星期的开始
%w 星期（0-6），星期天为 0，星期一为 1，以此类推。
%W 一年中的星期数（00-53）星期一为星期的开始
%x 本地相应的日期表示
%X 本地相应的时间表示
%Z 当前时区的名称
%% %号本身
```
方法
```python
import time

def trans_format(time_string, from_format, to_format='%Y.%m.%d %H:%M:%S'):
    time_struct = time.strptime(time_string, from_format)
    times = time.strftime(to_format, time_struct)
    return times

# 将11 May转为mm-dd形式
time_string = "11 May"
times = trans_format(time_string, '%d %b', '%m-%d')  # 由于没有输入年份，所以输出的默认年份是1900
> 05-11

# 将 %m/%d/%Y 转为 %Y-%m-%d 形式
time_string2 = "08/26/1988"
times2 = trans_format(time_string2, '%m/%d/%Y', '%Y-%m-%d')
print(times2)
> 1988-08-26
```

### 获取两个日期之间的所有日期
```python
# -*- coding: utf-8 -*-
from datetime import datetime, timedelta


def get_date_list(start_ymd=None, end_ymd=None):
    """
    获取日期列表
    :param start_ymd: 开始日期 type:datetime or str
    :param end_ymd: 结束日期 type:datetime or str
    :return:
    """
    if isinstance(start_ymd, str):
        start = datetime.strptime(str(start_ymd), "%Y%m%d")
    else:
        start = start_ymd
    if isinstance(end_ymd, str):
        end = datetime.strptime(str(end_ymd), "%Y%m%d")
    else:
        end = end_ymd

    def gen_dates(b_date, days):
        day = timedelta(days=1)
        for i in range(days):
            yield b_date + day * i

    if start is None:
        start = datetime.now() + timedelta(days=-2)
    if end is None:
        end = datetime.now()
    data = []

    for d in gen_dates(start, (end - start).days):
        data.append(d)
    return sorted(list(map(lambda x: x.strftime('%Y%m%d'), data)))


if __name__ == '__main__':
    start = datetime(2018, 5, 28)
    end = datetime(2018, 6, 2)
    date_list = get_date_list(start, end)
    date_list = get_date_list(datetime.strptime(str(20180528), "%Y%m%d"), datetime.strptime(str(20180602), "%Y%m%d"))
    print(date_list)
    
['20180528', '20180529', '20180530', '20180531', '20180601']    
```

### 封装成类
```python
# -*- coding: utf-8 -*-
"""
日期处理类
"""
import datetime
import time


class TimeFormat:
    def __init__(self):
        pass

    @classmethod
    def timestamp_2_datetime(cls, ans_time):
        """将unix时间戳转换为python的datetime"""
        return datetime.datetime.fromtimestamp(ans_time)

    @classmethod
    def timestamp_2_str(cls, ans_time):
        """将unix时间戳转换为时间字符串"""
        return time.strftime('%Y-%m-%d %H:%M:%S', time.gmtime(ans_time))

    @classmethod
    def datetime_2_timestamp(cls, d_time):
        """将python的datetime转换为unix时间戳"""
        return time.mktime(d_time.timetuple())

    @classmethod
    def datetime_2_str(cls, d_time):
        """将python的datetime转换为字符串"""
        return d_time.strftime('%Y-%m-%d %H:%M:%S')


if __name__ == '__main__':
    timestamp = 1617776979
    now_time = datetime.datetime.utcnow()
    print(TimeFormat.timestamp_2_str(timestamp))
    print(TimeFormat.timestamp_2_datetime(timestamp))
    print(TimeFormat.datetime_2_timestamp(now_time))
    print(TimeFormat.datetime_2_str(now_time))

> 2021-04-07 06:29:39
> 2021-04-07 14:29:39
> 1617930414.0
> 2021-04-09 09:06:54
```

### 其他时间处理模块

- pytz模块
- dateutil模块