---
title: MAC安装JDK8
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-03 17:25:26
img:
password:
summary: 安装jdk&配置环境变量
tags:
- mac
- jdk
- java
categories:
- 开发工具
---


### 安装
#### 下载`java8`安装包
前往[Oracle官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html )下载java8版本安装包 

当前演示版本为 jdk-8u291-macosx-x64.dmg
![SgHA5z](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/SgHA5z.png)

#### 安装`java8`
安装完成后查看版本信息
```
➜  Downloads java -version
java version "1.8.0_291"
Java(TM) SE Runtime Environment (build 1.8.0_291-b10)
Java HotSpot(TM) 64-Bit Server VM (build 25.291-b10, mixed mode)

```

### 配置环境变量

定位`JAVA_HOME`位置
```
➜  ~ /usr/libexec/java_home -V
Matching Java Virtual Machines (2):
    1.8.291.10 (x86_64) "Oracle Corporation" - "Java" /Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
    1.8.0_291 (x86_64) "Oracle Corporation" - "Java SE 8" /Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home
/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
```


配置`JAVA_HOME`环境变量, 编辑`~/.bash_profile`,添加
```
# java 
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home"
export CLASS_PATH="$JAVA_HOME/lib"
# 方式一
export PATH=".;$PATH:$JAVA_HOME/bin"
# 方式二
export PATH=:$JAVA_HOME/bin:$PATH
```

验证环境变量
```
export 
export $PATH
```
使配置生效
```
source ~/.zshrc
```
验证环境变量配置是否成功
```
$ echo $JAVA_HOME        
/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home
```



参考：
[macOS安装java8及环境变量配置](https://blog.csdn.net/irokay/article/details/71374426)