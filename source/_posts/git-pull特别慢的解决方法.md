---
title: git pull特别慢的解决方法
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-19 13:33:08
password:
summary:
tags:
- git
- git pull 
categories:
- Git
---

### 问题描述
- git pull 特别慢
- ssh -T `git@github.com `很慢


### 解决办法一
#### 查询GitHub域名ip
我们可以利用https://www.ipaddress.com/ 来获得以下两个GitHub域名的IP地址：

- github.com
- github.global.ssl.fastly.net

打开网页后，利用输入框内分别查询两个域名：

![github.com](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/Lfp4Xj.png)
![fastly.net](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/7rUMBA.png)


#### 修改hosts文件,刷新dns缓存
将以上两段IP写入Hosts文件中：

mac:
```
sudo vi /etc/hosts
# 添加
199.232.69.194 github.global.ssl.fastly.net
140.82.112.4 github.com
# 退出文件执行
sudo killall -HUP mDNSResponder
```

windows:
```
# hosts文件位置：
C:\windows\system32\drivers\etc

刷新dns：
win+r，输入CMD，回车

在命令行执行:ipconfig /flushdns #清除DNS缓存内容。
ps:ipconfig /displaydns//显示DNS缓存内容
```

### 解决办法二
由于看到github的两个IP属地为美国，因此将代理服务器切换至美国，并开启全局模式（实际验证PAC模式问题依旧），重新打开一个终端，git pull速度大幅提升