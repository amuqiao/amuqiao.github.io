---
title: Linux配置ssh免密登陆
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-31 16:58:55
img:
password:
summary:
tags:
- linux
- ssh
categories:
- 后端

---



> 三台机器机器实现互相免密码登录

<!--more-->

三台机器规划
![6ZP9e2](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/6ZP9e2.png)

1. 配置hostname与IP映射

修改hosts文件，在三台机器上增加以下配置
```
vi /etc/hosts

192.168.80.121 linux121
192.168.80.122 linux122
192.168.80.123 linux123
```
2. 在所有主机上创建⽬录并赋予权限
```
mkdir /root/.ssh 
chmod 700 /root/.ssh
```
3. 在三台机器执⾏以下命令，⽣成公钥与私钥
```
cd ~ #进⼊⽤户⽬录 
ssh-keygen -t rsa -P "" 

是⽣成ssh密码的命令，-t参数表示⽣成算法，有rsa和dsa两种；-P表示使⽤的密码，这⾥使⽤""空字 符串表示⽆密码。

cd ~/.ssh

进⼊.ssh

cat id_rsa.pub >> authorized_keys #这个命令将id_rsa.pub的内容追加到了authorized_keys的内容后⾯
```
![OrnCow](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/OrnCow.png)

4. 复制linux122机器的认证到其他机器
```
scp authorized_keys linux121:/root/.ssh/ 
scp authorized_keys linux123:/root/.ssh/
```

密码传输过程中只使⽤⼀次，以后再使⽤linux122 ssh linux121、linux123即不在需要密码，实现免密钥登录。

5. 三台机器互相免密登录
将三台机器的公钥添加到authorized_keys中，然后复制到三台机器.ssh目录下

6. mac 实现免密登录虚拟机
将mac ~/.ssh路径下的id_rsa.pub 追加到linux121、linux122、linux121 ~/.ssh/authorized_keys文件末尾

```
ssh root@192.168.80.121
```