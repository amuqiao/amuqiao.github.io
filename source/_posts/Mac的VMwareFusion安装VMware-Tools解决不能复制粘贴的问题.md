---
title: Mac的VMwareFusion安装VMware Tools解决不能复制粘贴的问题
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-31 10:34:10
img:
password:
summary:
tags:
- vmware fusion
categories:
- 开发工具

---


> 另一种思路：mac ssh 虚拟机也可以解决复制粘贴问题

<!--more-->

- 将VMTools拷贝到虚拟机中手动安装
- 在虚拟机中安装VMTools，缺少编译环境则安装编辑环境
- 在所需环境都安装完成后，执行VMTools安装
- 开启共享文件夹

/Applications/VMware Fusion.app/Contents/Library/isoimages
![qh9yVD](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/qh9yVD.png)
点击linux.iso
![qLgrZi](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/qLgrZi.png)
将MwareTools-10.3.23-17030940.tar.gz复制到Downloads文件夹
![CAnncG](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/CAnncG.png)

将VMTools拷贝到虚拟机中进行安装
```
scp ~/Downloads/VMwareTools-10.3.23-17030940.tar.gz root@192.168.80.121:/opt/lagou/software
scp ~/Downloads/VMwareTools-10.3.23-17030940.tar.gz root@192.168.80.122:/opt/lagou/software
scp ~/Downloads/VMwareTools-10.3.23-17030940.tar.gz root@192.168.80.123:/opt/lagou/software
```

解压
```
tar -zxvf VMwareTools-10.3.23-17030940.tar.gz ../
```
### 安装VMTools
```
cd vmware-tools-distrib
./vmware-install.pl 
-bash: ./vmware-install.pl: /usr/bin/perl: bad interpreter: No such file or directory
```

安装perl, gcc


```
yum -y update

yum -y install kernel-headers kernel-devel gcc perl
```

重启reboot 很重要！！！

环境安装后重复上面运行vmware-install.pl即可

```
sudo ./vmware-install.pl 
open-vm-tools packages are available from the OS vendor and VMware recommends 
using open-vm-tools packages. See http://kb.vmware.com/kb/2073803 for more 
information.
Do you still want to proceed with this installation? [no]
```
输入yes，然后一路回车，默认选项安装。

```
To enable advanced X features (e.g., guest resolution fit, drag and drop, and 
file and text copy/paste), you will need to do one (or more) of the following:
1. Manually start /usr/bin/vmware-user
2. Log out and log back into your desktop session
3. Restart your X session.
```

![WNqYPN](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/WNqYPN.png)

```
/usr/bin/vmware-user start ///手动启动
vmware-user: could not open /proc/fs/vmblock/dev
```
此错误未解决

可以设置共享目录

然后设置虚拟机共享目录后即可在/mnt/hgfs下看到共享目录

### 其他解决途径

> mac ssh vmware 虚拟机 解决复制粘贴问题

```
ssh 虚拟机用户名@虚拟机ip地址
输入密码
```


### 其他问题
#### yum -y install安装失败
安装编译环境

```
# yum -y install perl gcc make kernel-headers kernel-devel
yum -y update

yum -y install kernel-headers kernel-devel gcc perl
```
出现错误
```
...
http://mirrors.163.com/centos/7.2.1511/os/x86_64/repodata/repomd.xml: [Errno 14] curl#6 - "Could not resolve host: mirrors.163.com; Unknown error"
Trying other mirror.
...
```
一眼一看就知道是网络的问题，是不是没设置DNS，检查发现ping baidu.com不通，应该是DNS问题

修改DNS配置，将MAC的DNS地址添加进来
```
vi /etc/sysconfig/network-scripts/ifcfg-ens33

DNS1=xxxx
DNS2=xxxx

# 保存退出，重启网络服务
systemctl restart network 或者 service network restart
```
重启网络后，能正常ping通baidu.com

继续执行

#### 卸载VMTools

三、卸载：

到刚才解压的目录/usr/local/vmware-tools-distrib/bin 目录或者 /usr/bin

```
cd vmware-tools-distrib/bin

./vmware-uninstall-tools.pl    ///运行卸载程序

rm -rf /etc/vmware-caf     ///递归强制 删除残留目录

rm -rf /etc/vmware-tools   ///递归强制 删除残留目录

rm -rvf /usr/lib/vmware-tools     ///递归强制 删除残留目录

```
### 参考文档
- [VMware安装CentOS7踩坑](https://www.cnblogs.com/joxin/p/9700653.html)
- [【CentOS7】yum安装时出现错误Errno 14 Couldn't resolve host的解决办法](https://blog.csdn.net/yuesichiu/article/details/53351324)
- [VMwareTools安装及出现kernel header path的解决方法](https://blog.csdn.net/u013162548/article/details/45567681/)
- [Centos7最小化终端命令行安装VMware-tools.pl](https://blog.csdn.net/z69183787/article/details/79283313)
- [解决vmware fusion + centos 7安装vmtools时提示The path "" is not a valid path to the xxx kernel headers.](https://blog.csdn.net/sirchenhua/article/details/49719659)
- [Windows主机环境CentOS7虚拟机安装VMware Tools](https://finolo.gy/2020/01/Windows%E4%B8%BB%E6%9C%BA%E7%8E%AF%E5%A2%83CentOS7%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85VMware-Tools/)