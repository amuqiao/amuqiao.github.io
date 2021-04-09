---
title: Python项目配置文件
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-09 17:24:01
img:
password:
summary:
tags:
- python
- yaml
- 配置文件

categories:
- 后端

---


> Python 项目配置文件

### Yaml格式配置文件

#### 为什么使用YAML
- YAML的可读性好。
- YAML和脚本语言的交互性好。
- YAML使用实现语言的数据类型。
- YAML有一个一致的信息模型。
- YAML易于实现。

#### 首先安装yaml模块
```python
pip3 install pyyaml
```
#### 编写yaml配置文件 yaml_example.yaml
```yaml
app_config:
    log_level: 10

session_server:
    driver: redis
    driver_settings:
        host: localhost
        port: 6379
        password: 
        db: 1
        max_connections: 512

cache_server:
    driver: redis
    driver_settings:
        REDIS_HOST: localhost
        REDIS_PORT: 6379
        REDIS_PASSWORD: 
        REDIS_DB: 2
        REDIS_EX: 172800 
        REDIS_MAX_CONNECTIONS: 512

database:
    host: localhost
    port: 3306
    user: root
    password: 123456
    database: test
    charset: utf8

captcha_fonts:
    - fonts/RedHatDisplay-Regular.ttf
    - fonts/RedHatDisplay-Medium.ttf
    - fonts/RedHatDisplay-Black.ttf
    - fonts/RedHatDisplay-Bold.ttf

crypto_rsa:
    public_key_file: keys/rsa_pub_key.pem
    private_key_file: keys/rsa_pri_key.pem
```

#### 编写解析yaml文件的python程序 yaml_example.py
```python
import yaml

with open('local.yml') as f:
    content = yaml.load(f)
    print(type(content))
    print('before modification:', content)
    content['age'] = 17
    content['children'][1]['age'] = 1
    print('after modification', content)
```
#### 输出配置结果:
```python
<class 'dict'>
before modification: {'name': 'junxi', 'age': 18, 'spouse': {'name': 'Rui', 'age': 18}, 'children': [{'name': 'Chen You', 'age': 3}, {'name': 'Ruo Xi', 'age': 2}]}
after modification {'name': 'junxi', 'age': 17, 'spouse': {'name': 'Rui', 'age': 18}, 'children': [{'name': 'Chen You', 'age': 3}, {'name': 'Ruo Xi', 'age': 1}]}

```