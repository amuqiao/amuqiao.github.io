---
title: Prometheus入门教程
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-31
img:
password:
summary:
tags:
- Prometheus
- 数据库
- 监控
categories:
- 运维
---


> Prometheus 一款优秀的开源监控告警系统

<!--more-->

### Prometheus是什么
Prometheus 最开始是由 SoundCloud 开发的开源监控告警系统，是 Google BorgMon 监控系统的开源版本。能很好地与容器平台、云平台配合。

2016年继Kubernetes之后成为第二个正式加入CNCF基金会的项目，随着 Kubernetes 在容器编排领头羊地位的确立，Prometheus 也成为 Kubernetes 容器监控的标配。

#### 设计架构
监控系统的总体架构大多是类似的，都有数据采集、数据处理存储、告警动作触发和告警，以及对监控数据的展示。

下面是 Prometheus 的架构：
![Prometheus设计架构](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/Prometheus%E8%AE%BE%E8%AE%A1%E6%9E%B6%E6%9E%84.png)

Prometheus Server 负责定时从 Prometheus 采集端 Pull(拉) 监控数据。Prometheus 采集端可以是实现了 /metrics 接口的服务，可以是从第三方服务导出监控数据的 exporter，也可以是存放短生命周期服务监控数据的 Pushgateway。相比大多数采用 Push(推) 监控数据的方式，Pull 使得 Promethues Server 与被采集端的耦合度更低，Prometheus Server 更容易实现水平拓展。

对于采集的监控数据，Prometheus Server 使用内置时序数据库 TSDB 进行存储。同时也会使用这些监控数据进行告警规则的计算，产生的告警将会通过 Prometheus 另一个独立的组件 Alertmanager 进行发送。Alertmanager 提供了十分灵活的告警方式，并且支持高可用部署。

对于采集到的监控数据，可以通过 Prometheus 自身提供的 Web UI 进行查询，也可以使用 Grafana 进行展示。

#### Prometheus与Influxdb的区别

**prometheus是pull模型，influxdb是push模型。**

prometheus server启动的时候，你需要告诉它你想它监听哪些节点，每次增加或者减少节点，得重启prometheus server的。

而influxdb则是，每个监控的节点自己配置好influxdb server的地址，向服务端推送数据

对于Prometheus而言，监控节点不需要知道server在哪里，即使server挂了，对监控节点本身无影响

相对的，influxdb server如果挂了，那么所有influxdb 节点发送的请求都会出错

当然，prometheus可以通过push gateway转换成push模型，influxdb可以通过collector转化成pull模型

**influxdb的查询语句是类SQL，prometheus则不是**

Prometheus 是一套完整的监控系统，包括数据采集、数据处理存储、告警动作触发和告警，以及对监控数据的展示(数据展示很烂，官方都推荐使用Grafana)

influxdb 只是一个数据库

### 安装Prometheus
Prometheus基于Golang编写，编译后的软件包，不依赖于任何的第三方依赖。用户只需要下载对应平台的二进制包，解压并且添加基本的配置即可正常启动Prometheus Server。
#### 从二进制包安装
直接从[官网下载](https://prometheus.io/download/)

#### 使用Docker安装


### 安装Explorer采集数据

### 监控数据可视化

### 参考文档

- [Prometheus入门实践](https://juejin.cn/post/6844903938001469453)
- [Prometheus-book](https://yunlzheng.gitbook.io/prometheus-book/)
- [第01期：详解 Prometheus 专栏开篇](https://opensource.actionsky.com/20200427-prometheus/)
- [Prometheus数据采集](https://www.jianshu.com/p/6cc9a5046f17)