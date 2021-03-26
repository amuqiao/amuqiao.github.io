---
title: Docker使用手册
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-26 10:35:52
img:
password:
summary:
tags:
- Docker
categories:
- 运维

---

### 利用 Dockerfile 来创建镜像

使用 `docker commit` 来扩展一个镜像比较简单，但是不方便在一个团队中分享。我们可以使用 `docker build`来创建一个新的镜像。为此，首先需要创建一个 `Dockerfile`，包含一些如何创建镜像的指令。

新建一个目录和一个 `Dockerfile`
```
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile
```
Dockerfile 中每一条指令都创建镜像的一层，例如：
```
# This is a comment
FROM ubuntu:20.04.2
MAINTAINER Docker Newbee <newbee@docker.com>
RUN apt-get -qq update
RUN apt-get -qqy install ruby ruby-dev
RUN gem install sinatra
```

#### Dockerfile [基本的语法](http://www.dockerinfo.net/dockerfile%e4%bb%8b%e7%bb%8d)

- 使用`#`来注释
- `FROM` 指令告诉 `Docker` 使用哪个镜像作为基础
- 接着是维护者的信息
- `RUN`开头的指令会在创建中运行，比如安装一个软件包，在这里使用 `apt-get` 来安装了一些软件
编写完成 `Dockerfile` 后可以使用 `docker build` 来生成镜像。
```
# docker build -t="仓库名:tag" .
sudo docker build -t="ouruser/sinatra:v2" .
➜  sinatra docker build -t="ouruser/sinatra:v2" .
[+] Building 432.0s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                0.0s
 => => transferring dockerfile: 215B                                                                                                                                0.0s
 => [internal] load .dockerignore                                                                                                                                   0.0s
 => => transferring context: 2B                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/ubuntu:20.04.2                                                                                                   0.0s
 => [1/4] FROM docker.io/library/ubuntu:20.04.2                                                                                                                     0.0s
 => [2/4] RUN apt-get -qq update                                                                                                                                   33.8s
 => [3/4] RUN apt-get -qqy install ruby ruby-dev                                                                                                                   68.3s
 => [4/4] RUN gem install sinatra                                                                                                                                 327.8s
 => exporting to image                                                                                                                                              1.9s
 => => exporting layers                                                                                                                                             1.9s
 => => writing image sha256:32d826e013b38ee6a8d80fac491e9f39a59028252bf0aeeffb70db25bcd4a1c4                                                                        0.0s
 => => naming to docker.io/ouruser/sinatra:v2                   
 ➜  sinatra docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
ouruser/sinatra      v2        32d826e013b3   2 minutes ago   160MB
ubuntu               20.04.2   8e428cff54c8   4 hours ago     72.9MB
```
其中 -t 标记来添加 tag，指定新的镜像的用户信息。 “.” 是 Dockerfile 所在的路径（当前目录），也可以替换为一个具体的 Dockerfile 的路径。

可以看到 build 进程在执行操作。它要做的第一件事情就是上传这个 Dockerfile 内容，因为所有的操作都要依据 Dockerfile 来进行。 然后，Dockfile 中的指令被一条一条的执行。每一步都创建了一个新的容器，在容器中执行指令并提交修改（就跟之前介绍过的 docker commit 一样）。当所有的指令都执行完毕之后，返回了最终的镜像 id。所有的中间步骤所产生的容器都被删除和清理了。

*注意一个镜像不能超过 127 层

此外，还可以利用 ADD 命令复制本地文件到镜像；用 EXPOSE 命令来向外部开放端口；用 CMD 命令来描述容器启动后运行的程序等。例如

```
# put my local web site in myApp folder to /var/www
ADD myApp /var/www
# expose httpd port
EXPOSE 80
# the command to run
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
```

#### 其他配置示例
```
FROM python:3.7-alpine 
# WORKDIR 工作路径
WORKDIR /app
# COPY 复制文件
COPY requirements.txt /app
# RUN 执行命令
RUN pip3 install -r requirements.txt --no-cache-dir
COPY . /app 
ENTRYPOINT ["python3"] 
# 
CMD ["app.py"]

```

### docker run 常用参数
```
-i	以交互模式运行容器，通常与 -t 同时使用；
-t	为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-d	后台运行容器，并返回容器ID；
```

### 参考文档
- [Docker入门教程](https://www.docker.org.cn/book/docker/what-is-docker-16.html)
- [Docker中文文档](http://www.dockerinfo.net/document)
- [Docker入门与实践](https://hujb2000.gitbooks.io/docker-flow-evolution/content/cn/index.html)