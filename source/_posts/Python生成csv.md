---
title: Python生成csv
top: false
cover: false
toc: true
mathjax: true
date: 2022-07-03 01:48:09
img:
password:
summary: 脚本处理csv
tags:
- csv
- python
categories:
- 后端

---

### 写csv
```
import csv
ymds = ['20180611', '20180612']
data = {
    '20180611':{
        'name': 'tom',
        'score': 90
    },
    '20180612':{
        'name': 'tom',
        'score': 95
    }
}
with open('test.csv', 'w') as f:
    title_list = ['date', 'name', 'score']
    fieldnames = title_list  # 标题名称
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    
    for ymd in ymds:
        row = {
            'date': ymd,
            'name': data[ymd]['name'],
            'score': data[ymd]['score']
        }
        writer.writerow(row)  # 按行写入
    
```
### 读csv
```
def read_csv(file_name):
    line = []
```