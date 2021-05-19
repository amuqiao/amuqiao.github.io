---
title: Grafana配置手册
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-19 09:43:45
img:
password:
summary:
tags:
- grafana
categories:
- 运维

---


> 一个开源的指标量监测和可视化工具

<!--more-->

Grafana是一个开源的指标量监测和可视化工具。常用于展示基础设施的时序数据和应用程序运行分析。Grafana的dashboard展示非常炫酷，绝对是运维提升逼格的一大利器。

官方在线的demo可以在这里找到: http://play.grafana.org/
![grafana示例](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/3uhNgK.jpg)

### 安装
Grafana的安装非常简单，官方就有软件仓库可以直接使用，也可以通过docker镜像等方式直接本地启动。

参考官方文档的安装方法，对应找到你的操作系统的安装方法即可: http://docs.grafana.org/

>值得一提的是，由于官方仓库托管在S3上，国内用户直接访问苦不堪言，万幸的是清华大学tuna镜像站已经提供了grafana的镜像，只需要将官方文档中提到的仓库地址对应的换成清华大学的镜像站的地址即可: https://mirrors.tuna.tsinghua.edu.cn/grafana/

安装完毕后，使用service grafana-server start就可以启动grafana，访问http://your-host:3000就可以看到登录界面了。

默认的用户名和密码都是admin。

>默认情况下，grafana的配置存储于sqlite3中，如果你想使用其他存储后端，如mysql，postgresql等，请参考官方文档配置: http://docs.grafana.org/installation/configuration/

grafana的几个基本构成和基本概念参考官方文档: http://docs.grafana.org/guides/basic_concepts/ ，后面会逐步提到这些关键词。

### 配置数据源
先要明确一点，grafana只是一个dashboard(4版本开始将引入报警功能)，负责把数据库中的数据进行可视化展示，本身并不存储任何数据，另外某些查询和聚合使用的是数据库本身提供的功能，需要对应的数据库去支持。因此不代表你换了一个数据库后端就一定能展示相同的图形。

grafana目前支持的时序数据库有: Graphite, Prometheus, Elasticsearch, InfluxDB, OpenTSDB, AWS Cloudwatch。未来可能会有更多的数据库的支持加入，请关注更新。也可以使用第三方插件引入支持。

我们这里使用Elasticsearch作为数据库的来源。


### 参考文档
- [ELKstack 中文指南Grafana【详细】](https://elkguide.elasticsearch.cn/elasticsearch/other/grafana.html)