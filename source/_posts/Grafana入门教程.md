---
title: Grafana入门教程
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-22
img:
password:
summary:
tags:
- grafana
categories:
- 运维
---

> [Grafana](https://github.com/grafana/grafana)是一个开源的度量分析与可视化套件。<!--more--> 经常被用作基础设施的时间序列数据和应用程序分析的可视化，它在其他领域也被广泛的使用包括工业传感器、家庭自动化、天气和过程控制等。

### Grafana 是什么
Grafana 是一个监控仪表系统，它是由 Grafana Labs 公司开源的的一个系统监测 (System Monitoring) 工具。它可以大大帮助你简化监控的复杂度，你只需要提供你需要监控的数据，它就可以帮你生成各种可视化仪表。同时它还有报警功能，可以在系统出现问题时通知你。

Grafana 不对数据源作假设，它支持以下各种数据，也就是说如果你的数据源是以下任意一种，它都可以帮助生成仪表。同时在市面上，如果 Grafana 称第二，那么应该没有敢称第一的仪表可视化工具了。因此，如果你搞定了 Grafana，它几乎是一个会陪伴你到各个公司的一件称心应手的兵器。

#### Grafana 支持的数据源

- Prometheus 时序数据库 本文中的例子，你没听过也没关系不影响阅读，把它想象成带时间戳的 MySQL 就好
- Graphite
- OpenTSDB
- InfluxDB
- MySQL/PostgreSQL
- Microsoft SQL Server

### 安装和配置 Grafana
> 为了简化各种系统不一致的乱七八糟问题，我们用 Docker 来安装 Grafana。

请 clone [本教程代码](https://github.com/amuqiao/grafana-tutorial/tree/main)，然后确认本地已经安装 `docker-compose` 后，在本目录运行
```
docker-compose up
```

这条命令会启动三个 docker 容器，`prometheus`, `service` 和 `grafana`
- prometheus 是普罗米修斯时序数据库
- service 是普罗米修斯自带的数据生成器（监控本机 CPU 内存等信息）
- grafana 就是 Grafana 服务本身

在你的浏览器中访问 [localhost:3000](localhost:3000) 即可看到运行的 Grafana，再根据教程中的步骤设置好源即可

### 配置仪表盘
在使用`docker-compose up`启动服务后，到你的浏览器中，复制 [http://localhost:3000](http://localhost:3000) 应该就可以看到 Grafana 跑起来的初始登录界面。初始的用户名是 admin，密码也是 admin。输入之后，会要求你改密码,这里我改为123456
![22B15n](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/22B15n.png)

然后就可以进入 Grafana 的主界面了：
![eNTG0T](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/eNTG0T.png)

到这里，你的 Grafana 就已经搭起来了。注意到 Docker 的配置文件中我们创建了三个服务，这三个服务之间分别有什么关系呢？

或者说，Grafana 和时序数据库，数据源之间有什么关系呢？请看下文 Grafana 工作原理

### Grfana 工作原理
上面说到，Grafana 是一个仪表盘，而仪表盘必然是用来显示数据的。

Grafana 本身并不负责数据层，它只提供了通用的接口，让底层的数据库可以把数据给它。而我们起的另一个服务，叫 Prometheus （中文名普罗米修斯数据库）则是负责存储和查询数据的。

也就是说，Grafana 每次要展现一个仪表盘的时候，会向 Prometheus 发送一个查询请求。

那么配置里的另一个服务 Prometheus-exporter 又是什么呢？

这个就是你真正监测的数据来源了，Prometheus-exporter 这个服务，会查询你的本地电脑的信息，比如内存还有多少、CPU 负载之类，然后将数据导出至普罗米修斯数据库。

在真实世界中，你的目的是监控你自己的服务，比如你的 Web 服务器，你的数据库之类。

那么你就需要在你自己的服务器中把数据发送给普罗米修斯数据库。当然，你完全可以把数据发送给 MySQL (Grafana 也支持)，但普罗米修斯几乎是标配的时序数据库，强烈建议你用。

用[一张图](https://shahbhargav.medium.com/monitoring-docker-containers-using-cadvisor-and-prometheus-5350ae038f45)来说明它们之间的关系:
![hq0RtW](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/hq0RtW.jpg)

这里，最左边的 Docker 服务会将服务的数据发送给中间的普罗米修斯（对应上文的 Prometheus-exporter），而最右边的 Grafana 会查询中间的普罗米修斯，来展示仪表盘。

### 搭建你的第一个仪表盘
#### 设置数据源

进入 Grafana 后，在左侧你会发现有一个 Data Source 即数据源选项
![c6T5xz](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/c6T5xz.png)
点击后进入，点 Add Data Source 即添加数据源，选择 Prometheus
![nTuJVL](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/nTuJVL.png)

之后设置数据源 URL。请注意，Promethues 的工作原理是通过轮询一个 HTTP 请求来获取数据的，而 Grafana 在获取数据源的时候也是通过一个 HTTP 请求，因此这个地方你需要告诉 Grafana 你的 Prometheus 的数据端点是什么。

这里我们填入 http://prometheus:9090 就可以了。
![ohtJiw](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/ohtJiw.png)
![xXyiQB](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/xXyiQB.png)
点确认时一定要确认出现 Data source is working 这个检测，这时表明你的 Grafana 已经跟普罗米修斯说上话了

#### 导入 Dashboard
在 Grafana 里，仪表盘的配置可以通过图形化界面进行，但配置好的仪表盘是以 JSON 存储的。这也就是说，如果你把你的 JSON 数据分享出去，别人导入就可以直接导入同样的仪表盘（前提是你们的监测数据一样）。

对于我们的例子来说，回忆一下，因为我们用了 prometheus-exporter 也就是本机的系统信息监控，那么我们可以先找一个同样用了这个数据源的仪表盘。在 Grafana 网站上，你其实可以找到很多别人已经做好的仪表，可以用来监测非常多标准化的服务。

Grafana 的仪表盘市场：https://grafana.com/grafana/dashboards

比如说针对以下一些服务的标准仪表盘就可以在这里找到

- JVM
- Spring Boot
- MySQL 监控
- Laravel 监控
那么，这里我们就用一个标准的仪表盘：https://grafana.com/grafana/dashboards/1860

在左侧的加号里，点 Import 即导入，在出现的界面中填入 1860 即我们要导入的仪表盘编号即可。
![j198Ty](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/j198Ty.png)
然后填入你需要的信息，比如仪表盘名字等
![J6HLTl](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/J6HLTl.png)
确认之后 Grafana 就会根据你的本机信息，生成类似 CPU 负载，内存和 I/O 之类的信息。我的磁盘状况如图：
![HZrRYJ](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/HZrRYJ.png)
要注意的是，这里的信息真正监控的是你的 Docker 中的系统信息。如果你只给你的 Docker 分配 1 个核和 2G 内存，那么这里应该看到的就是 1 个核和 2G 内存
####  生成和创建新的仪表盘
最后，在上面导入信息的基础上，你就可以开始创建和你的服务、业务相关的仪表盘了。

但在这步之前，你需要先在你的服务中开始记录一些数据。

这一步涉及

**如何在各种语言中记录你关注的数据？**

拿卡拉搜索举例子，我们关注用卡拉搜索的 APP 的搜索响应速度，所以自然我们需要在所有搜索请求处记录延迟。

对于你的服务，你需要根据自己的业务确认哪些数据是重要的，关于如何记录数据，如何思考运维等，我会在之后的博客中继续深入讨论。如果你想更深入地了解这个领域，推荐阅读 Google 运维手册这本书，英文名叫《Google SRE Book》，免费的书。

**包括如何用 Prometheus 查询数据**

普罗米修斯本身也是个非常大的话题，我们会在之后的博客中继续讨论。普罗米修斯包括所有其它时序数据库通常都会定义一个查询语言，比如说 PromQL，如果需要熟练地构建仪表盘的话，需要对这个查询语言有一定了解。

**如何手动生成一个仪表盘**

假设你已经按上面的步骤生成了一个基本的仪表盘，那么现在可以开始手动添加仪表盘了。同样是点左侧的加号，点 Dashboard 就可以进入添加仪表盘的界面。

这里我们选择一个数据叫 scrape_duration_seconds，先不用管它的含义是什么，就当它是双 11 的销售额好了：
![OHf5oL](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/OHf5oL.png)
添加好后点右上角的 Apply 或 Save 你的仪表盘就被保存了。这时候，用一个大屏幕展示一下，庆祝一下双十一又过了千亿吧 :D

### 总结
这篇文章里我们从头到尾介绍了如何用 Grafana 生成仪表盘，如何配置和连接数据源，以及如何导入和创建一个仪表盘。

### 参考文档
- [Grafana 管理员手册](https://support.websoft9.com/docs/grafana/zh/)
- [Grafana 教程 - 构建你的第一个仪表盘](https://kalasearch.cn/blog/grafana-with-prometheus-tutorial/)