---
title: MAC配置环境变量
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-03 16:10:50
img:
password:
summary: 配置环境变量
tags:
- mac
categories:
- 后端

---
编辑~/.zshrc

```
vi ~/.zshrc
# 设置别名
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# 添加docker/bin路径
# export PATH=${PATH}:/Applications/Docker.app/Contents/Resources/bin; 

# 语法 export PATH=${PATH}:相关目录路径;


# hadoop
export HADOOP_HOME=/usr/local/servers/hadoop-2.9.2
export PATH=:$HADOOP_HOME/bin:$PATH
```
配置完立即生效
```
source ~/.zshrc
```