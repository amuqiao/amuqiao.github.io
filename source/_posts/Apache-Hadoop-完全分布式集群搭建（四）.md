---
title: Apache Hadoop 完全分布式集群搭建（四）
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-04 11:03:29
img:
password:
summary: 其他配置
tags:
- hadoop
categories:
- 大数据

---

### 其他配置
#### 修改副本数量
在configuration中新增配置，并同步到其他节点
```
vi hdfs-site.xml

<configuration> 
    <property> 
        <name>dfs.replication</name> 
        <value>1</value> 
    </property> 
</configuration>
```
参数优先级

参数优先级排序：（1）代码中设置的值 >（2）用户自定义配置文件 >（3）服务器的默认配置