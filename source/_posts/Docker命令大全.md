---
title: Docker命令大全
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-26 10:35:52
img:
password:
summary:
tags:
- docker
categories:
- 运维

---

### 容器生命周期管理
- run
- start/stop/restart
- kill
- rm
- pause/unpause
- create
- exec
### 容器操作
- ps
- inspect
- top
- attach
- events
- logs
- wait
- export
- port
### 容器rootfs命令
- commit
- cp
- diff
### 镜像仓库
- login
- pull
- push
- search
### 本地镜像管理
- images
- rmi
- tag
- build
- history
- save
- load
- import
- info|version
- info
- version




### 案例
#### docker镜像列表存在但删除显示 No such image问题解决
1. 停掉容器（docker stop 容器ID或容器名称）
2. 删除容器（docker rm 容器ID或容器名称）
3. 删除镜像（docker rmi 镜像ID或镜像名称:版本号）

#### docker run 启动容器
```
-i	以交互模式运行容器，通常与 -t 同时使用；
-t	为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-d	后台运行容器，并返回容器ID；
```


### 参考文档
- [Docker 命令大全](https://www.runoob.com/docker/docker-command-manual.html)