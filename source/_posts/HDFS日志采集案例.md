---
title: HDFS日志采集案例
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-11 15:38:49
img:
password:
summary: 需求分析+代码实现
tags:
- hadoop
- log
categories:
- 大数据

---


![G4Qbv4](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/G4Qbv4.png)
### 需求分析

- 定时采集已滚动完毕日志文件 
- 将待采集文件上传到临时目录 
- 备份日志文件

### 代码实现

### 验证调优