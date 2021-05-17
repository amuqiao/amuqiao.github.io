---
title: Linux常用命令整理
top: false
cover: false
toc: true
mathjax: true
date: 2021-04-29 09:05:00
img:
password:
summary:
tags:
- linux
- shell

categories:
- 后端

---


> Linux 常用命令整理

<!--more-->

### 端口占用
lsof -i:端口号 用于查看某一端口的占用情况，比如查看8000端口使用情况，lsof -i:8000
```
# lsof -i:端口号
➜  lsof -i:8001

COMMAND   PID     USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
Python  12594 wangqiao    5u  IPv4 0xe79196fc978d9f53      0t0  TCP *:vcom-tunnel (LISTEN)

```
netstat -tunlp |grep 端口号，用于查看指定的端口号的进程情况，如查看8000端口的情况，netstat -tunlp |grep 8000

```
# netstat -tunlp 
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name   
tcp        0      0 0.0.0.0:111                 0.0.0.0:*                   LISTEN      4814/rpcbind        
tcp        0      0 0.0.0.0:5908                0.0.0.0:*                   LISTEN      25492/qemu-kvm      
tcp        0      0 0.0.0.0:6996                0.0.0.0:*                   LISTEN      22065/lwfs          
tcp        0      0 192.168.122.1:53            0.0.0.0:*                   LISTEN      38296/dnsmasq       
tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN      5278/sshd           
tcp        0      0 127.0.0.1:631               0.0.0.0:*                   LISTEN      5013/cupsd          
tcp        0      0 127.0.0.1:25                0.0.0.0:*                   LISTEN      5962/master         
tcp        0      0 0.0.0.0:8666                0.0.0.0:*                   LISTEN      44868/lwfs          
tcp        0      0 0.0.0.0:8000                0.0.0.0:*                   LISTEN      22065/lwfs
```
```
# netstat -tunlp | grep 8000
tcp        0      0 0.0.0.0:8000                0.0.0.0:*                   LISTEN      22065/lwfs 
```

### 磁盘清理
```
du -sh
du -sh *
df -h
du -sh * grep G
ll 
ll -h
```