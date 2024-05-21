---
title: Mac下安装JDK1.8和JDK11双版本并任意切换
top: false
cover: false
toc: true
mathjax: true
date: 2023-02-14 16:25:44
img:
password:
summary: mac下jdk8和jdk11切换
tags:
- jdk
categories:
- Java

---


1.官网下载JDK8和JDK11安装包，安装后打开bash,可以看到2个版本安装成功

```java
XXX-MacBook-Pro:~ chunyanzhang$ cd /Library/Java/JavaVirtualMachines
XXX-MacBook-Pro:JavaVirtualMachines chunyanzhang$ ls -al
total 0
drwxr-xr-x  4 root  wheel  128 12 18 00:18 .
drwxr-xr-x  4 root  wheel  128 11 15 17:25 ..
drwxr-xr-x  3 root  wheel   96 11 15 17:25 jdk-11.0.2.jdk
drwxr-xr-x  3 root  wheel   96 12 18 00:18 jdk1.8.0_271.jdk
```

2.编辑环境变量
```java
xport JAVA_11_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_271.jdk/Contents/Home
alias jdk8='export JAVA_HOME=$JAVA_8_HOME'
alias jdk11='export JAVA_HOME=$JAVA_11_HOME'
export JAVA_HOME=$JAVA_8_HOME

export ANDROID_HOME=/Users/chunyanzhang/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

alias python="/Library/Frameworks/Python.framework/Versions/3.11/bin/python3"
```

3.：wq! 保存并退出

4.执行使配置的环境变量生效 source ～/.bash_profile

5.查看当前jdk版本，使用命令： java -version
```java
XXX-MacBook-Pro:JavaVirtualMachines chunyanzhang$ java -version
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```

6.在jdk8和jdk11之间切换(默认jdk8)

- 从jdk8切换到jdk11
```java
XXX-MacBook-Pro:JavaVirtualMachines chunyanzhang$ jdk11
XXX-MacBook-Pro:JavaVirtualMachines chunyanzhang$ java -version
java version "11.0.2" 2019-01-15 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.2+9-LTS, mixed mode)

```

- 从jdk11切换到jdk8
```java
XXX-MacBook-Pro:JavaVirtualMachines chunyanzhang$ jdk8
XXX-MacBook-Pro:JavaVirtualMachines chunyanzhang$ java -version
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```
