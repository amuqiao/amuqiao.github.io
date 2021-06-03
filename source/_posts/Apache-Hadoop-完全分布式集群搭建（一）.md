---
title: Apache Hadoop 完全分布式集群搭建（一）
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-20 14:43:43
img:
password:
summary:
tags:
- hadoop
categories:
- 大数据

---

> 第一部分:虚拟机环境准备

<!--more-->

目标：
- 三台虚拟机（静态IP，关闭防火墙，修改主机名，配置免密登录，集群时间同步）(admin:hadoop)
- 在/opt目录下创建文件夹
    ```
    mkdir -p /opt/lagou/software  # 软件安装包存放目录
    mkdir -p /opt/lagou/servers   # 软件安装目录
    ```
- [Hadoop-2.9.2下载地址](https://archive.apache.org/dist/hadoop/common/hadoop-2.9.2/)
- [Hadoop最新下载地址](https://hadoop.apache.org/releases.html)
- [Hadoop官网地址](http://hadoop.apache.org/)
- 上传hadoop安装文件到/opt/lagou/software
    ```
    scp ~/Downloads/hadoop-2.9.2.tar.gz root@192.168.80.121:/opt/lagou/software
    scp ~/Downloads/hadoop-2.9.2.tar.gz root@192.168.80.122:/opt/lagou/software
    scp ~/Downloads/hadoop-2.9.2.tar.gz root@192.168.80.123:/opt/lagou/software
    ```

<!--more-->
### Hadoop搭建方式

- 单机模式：单节点模式，非集群，生产不会使用这种方式
- 单机伪分布式模式：单节点，多线程模拟集群的效果，生产不会使用这种方式
- 完全分布式模式：多台节点，真正的分布式Hadoop集群的搭建（生产环境建议使用这种方式）


### 环境准备

> 目标：使用vmware虚拟机虚拟三台linux节点，linux操作系统：Centos7

- [JDK8版本下载](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)（Hadoop框架是采用Java语言编写，需要java环境（jvm））
- [VMware Fusion 12  pro 下载地址](https://my.vmware.com/web/vmware/downloads/info/slug/desktop_end_user_computing/vmware_fusion/12_0?wd=&eqid=935f58bd0000e0bb0000000660a60ca6)
- [Hadoop-2.9.2下载地址](https://archive.apache.org/dist/hadoop/common/hadoop-2.9.2/)

> 虚拟机软件使用 VMware Fusion 12


### 概述
第一部分
1. 安装软件VMware Fusion 12 
2. 在虚拟机上安装CentOS7
3. 准备三台虚拟机（静态IP，关闭防火墙，修改主机名，配置免密登录，集群时间同步）

第二部分
1. 集群规划
1. 安装Hadoop
1. 启动集群

第三部分
1. 集群测试
2. 配置日志历史服务器

### VMware Fusion 12.1.2 激活码
```
ZF3R0-FHED2-M80TY-8QYGC-NPKYF
YF390-0HF8P-M81RQ-2DXQE-M2UT6
ZF71R-DMX85-08DQY-8YMNC-PPHV8
```

### 安装VMware Fusion 12和CentOS7
> 本示例VMware Fusion使用版本12.1.2，目前生产环境大多数用CentOS7
- [VMware Fusion 12.1.2 下载地址](https://my.vmware.com/web/vmware/downloads/info/slug/desktop_end_user_computing/vmware_fusion/12_0?wd=&eqid=935f58bd0000e0bb0000000660a60ca6)
- [CentOS7 下载地址](https://www.centos.org/centos-linux/)(选择7(2009)->x86_64->选择一个镜像站点->选择`CentOS-7-x86_64-DVD-2009.iso`下载即可)


![azhlDs](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/azhlDs.png)

默认容量20GB，默认内存1GB
![hGVLLt](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/hGVLLt.png)

自定义参数
 - 修改内存为2048MB
 - 处理器数量1
![内存设置](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/jREAbM.png)

![选择网络类型](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/uES73T.png)

磁盘大小一般建议20G，将虚拟磁盘拆分为多个文件
![磁盘类型](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/OXN98Y.png)
启动磁盘，安装CentOS7
![p4qjjw](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/p4qjjw.png)

配置语言环境，选择English，继续
![设置语言](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/QD3x8A.png)

配置时区，点击DATE&TIME，地图选择中国，时区填上海，点击左上角Done完成配置
![设置时区](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/tGcbHs.png)

选择INSTALLATION DESTINATION，选择`I will configure partitioning`自定义分区,左上角Done
![dGp3Qg](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/dGp3Qg.png)
点击+号
![IDzSv7](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/IDzSv7.png)
![V0DiUY](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/V0DiUY.png)
点击+ 添加swap分区
![wAUkjH](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/wAUkjH.png)
点击+，添加根分区，不设置分区大小，把剩余空间都给根分区
![OGxEVZ](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/OGxEVZ.png)
点击Done完成分区设置，点击Accept Changes 完成设置
![QKUuxA](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/QKUuxA.png)

此时返回上一页，点击Begin Installation开始安装
![eXml8J](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/eXml8J.png)

创建密码，密码为hadoop，不创建用户，默认用户root
![RMPTvL](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/RMPTvL.png)

设置完密码会显示Performing post-installation setup tasks
![2RUabL](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/2RUabL.png)

重启
![重启](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/E7tlaP.png)

### 给虚拟机配置静态ip地址

1. 在VMware Fusion的网络配置中新增网络连接，允许使用NAT，并配置自定义网段
2. 在本机终端中，查看配置好的NAT网段的网关ip
3. 给虚拟机配置静态ip地址和NAT网关地址，配置主机的DNS解析地址(mac-网络-高级-DNS查看ip)
4. 验证网络是否通畅(ping 网关ip，ping dns ip，ping baidu.com) 

#### 1.新建NAT网络
VMware Fusion>偏好设置>网络> 点击+ 新增网络连接

- 允许使用NAT
- 配置IP网段

这里主要需要手动配置子网ip，我将它自定义为了：192.168.100.0
![新建NAT网络](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/pKpmgD.png)

#### 2.查看vnet配置
```
cat /Library/Preferences/VMware\ Fusion/networking

...
answer VNET_4_DHCP yes
answer VNET_4_HOSTONLY_NETMASK 255.255.255.0  # 这个是子网掩码
answer VNET_4_HOSTONLY_SUBNET 192.168.100.0 # 这个是子网地址
answer VNET_4_NAT yes
answer VNET_4_NAT_PARAM_UDP_TIMEOUT 30
answer VNET_4_VIRTUAL_ADAPTER yes
...
```
#### 3.查看vnet4的nat配置

> VMware Fusion 文件夹有空格,vmnet可以替换成其他网络连接名称

主要是查看NAT网段的网关ip
```
cat /Library/Preferences/VMware\ Fusion/vmnet4/nat.conf

...
# NAT gateway address
ip = 192.168.100.1
netmask = 255.255.255.0

# Last DHCP address
lastDhcpAddress = 192.168.100.127

...
```
#### 4.修改虚拟机网络连接
> 虚拟机>网络适配器>网络适配器 设置...
> 这里我们选择新增的选择vmnet4
> ![HaOZPU](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/HaOZPU.png)

#### 5.修改虚拟机配置文件
> 虚拟机中的配置

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

原始配置
```
TYPE=Ethernet
BOOTPROTO=dhcp  # 需要修改为 static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
NAME=ens33
DEVICE=ens33
ONBOOT=no  # 需要修改为 yes
```

配置静态ip地址

```
TYPE=Ethernet
BOOTPROTO=static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
NAME=ens33
DEVICE=ens33
ONBOOT=yes

IPADDR=192.168.100.2      // 自定义的虚拟机IP， ip属于vmnet4网段
GATEWAY=192.168.100.1      // 配置的vmnet4网关地址
NETMASK=255.255.255.0       // 配置的掩码
DNS=192.168.8.1        // macOS本机的DNS地址。 系统偏好设置-> 网络 -> 在左侧选择当前使用的网络，点击右下角的“高级”按钮 -> 切换Tab页，可找到DNS地址。
若有多个DNS地址，则用DNS1，DNS2 区分

# 保存退出，重启网络服务
systemctl restart network 或者 service network restart
```

#### 7.查看IP是否配置成功
```
ip addr 

# 查看DNS配置 修改后立即生效，不需要重启服务
/etc/resolv.conf  
```
![查看IP是否配置成功](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/19LS8C.png)
#### 8.验证网络
- 虚拟机 ping vmnet4 网关
- 主机 ping 虚拟机
- 虚拟机 ping dns ip
- 虚拟机 ping baidu.com

![Dthyn9](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/Dthyn9.png)
重启VMware 网络

```
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start

```

#### 关闭CentOS7防⽕墙

防⽕墙相关的命令：
```
systemctl status ﬁrewalld.service # 查看ﬁrewall状态 
systemctl stop ﬁrewalld.service # 停⽌ﬁrewall 
systemctl disable ﬁrewalld.service # 禁⽌ﬁrewall开机启动
```

### 修改虚拟机名称和hostname

修改虚拟机名称 右键重命名即可
![修改虚拟机名称](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/54Wlxo.png)

修改hostname

```
vi /etc/sysconﬁg/network 打开network编辑

# Create by anaconda
NETWORKING=yes  # 新增
hostname=linux121 # 新增

```
保存退出后输⼊hostname,即可显示出centos121

注意：改动配置⽂件后要进⾏⽹络重启  或重启init 6 从⽽使配置⽂件⽣效
```
service network restart
```

### 配置hostname与IP映射

```
vi /etc/hosts

# 新增
192.168.80.121 linux121
192.168.80.122 linux122
192.168.80.123 linux123
```
我此时已经配置了三台机器的IP与hostname映射，从⽽可以达到，在本机ping hostname可通，如果 没有配此映射，需⽤ping IP地址可通。如果是三台机器互相⽤hostname来ping，那么三台机器必须同 时配好三个IP和hostname的映射。

⾄此，⽹络配置完成。

### 克隆并配置⽹络
三台机器规划
![6ZP9e2](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/6ZP9e2.png)

在linux122 上ping linux121 成功

![DLXVfk](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/DLXVfk.png)

### ⼤数据集群环境准备
#### 三台虚拟机关闭防⽕墙
```
查看防⽕墙状态 
systemctl status ﬁrewalld
停用防火墙
systemctl stop firewalld
开机自动关闭防火墙
systemctl disable firewalld
```
#### 三台机器关闭selinux
```
vi /etc/selinux/config
```
![iMh0B3](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/iMh0B3.png)

#### 三台机器机器免密码登录
1. 配置hostname与IP映射
pass
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
![uNJHSM](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/uNJHSM.png)

4. 复制第⼀台机器的认证到其他机器
```
scp authorized_keys linux121:/root/.ssh/ 
scp authorized_keys linux123:/root/.ssh/
```

密码传输过程中只使⽤⼀次，以后再使⽤ssh linux121或ssh linux123即不在需要密码，实现免密钥登录。

#### 三台机器时钟同步
时间同步的⽅式：在集群中找⼀台机器，作为时间服务器。


```
通过⽹络连接外⽹进⾏时钟同步,必须保证虚拟机连上外⽹

ntpdate us.pool.ntp.org; 阿⾥云时钟同步服务器 
ntpdate ntp4.aliyun.com
```
集群中其他机器与这台机器定时的同步时间，⽐如，每 隔⼗分钟，同步⼀次时间。

1. 时间服务器配置（必须root⽤户）

第⼀步:确定是否安装了ntpd的服务
> NTP 是网络时间协议(Network Time Protocol)，它是用来同步网络中各个计算机的时间的协议。
> ntpd 是网络时间协议 (NTP) 的守护进程

```
如果没有安装,可以进行在线安装
yum -y install ntp 启动ntpd的服务 
service ntpd start 设置ntpd的服务开机启动 
chkconfig ntpd on
第⼀步:确定是否安装了ntpd的服务 
rpm -qa | grep ntpd
```

第⼆步:编辑/etc/ntp.conf


```
编辑第⼀台机器的/etc/ntp.conf
vim /etc/ntp.conf 
在⽂件中添加如下内容
restrict 192.168.80.0 

注释⼀下四⾏内容 
#server 0.centos.pool.ntp.org 
#server 1.centos.pool.ntp.org 
#server 2.centos.pool.ntp.org 

#server 3.centos.pool.ntp.org 去掉以下内容的注释，如果没有这两⾏注释，那就⾃⼰添加上 
server 127.127.1.0 # local clock 
fudge 127.127.1.0 stratum 10
```
![frJchA](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/frJchA.png)
配置以下内容，保证BIOS与系统时间同步 vim /etc/sysconﬁg/ntpd 添加⼀⾏内容


```
SYNC_HWLOCK=yes
```
第三步：重新启动ntpd

```
service ntpd status
```
若ntpd 已停

```
service ntpd start
```
使NTP服务可以在系统引导的时候⾃动启动 ：

```
chkconfig ntpd on
```

2. 其他机器配置（必须root⽤户）

第⼀步：在其他机器配置10分钟与时间服务器同步⼀次


```
crontab -e
```
编写脚本 使另外两台机器与192.168.80.121进⾏时钟同步

```
*/10 * * * * /usr/sbin/ntpdate 192.168.80.121
```
第⼆步：修改任意机器时间

```
date -s "2020-06-20 11:11:11"
```
第三步：⼗分钟后查看机器是否与时间服务器同步

```
date
```

#### 三台机器安装jdk

查看⾃带的openjdk


```
rpm -qa | grep java
```


如果有⾃带的，卸载系统⾃带的openjdk

```
rpm -e java-1.6.0-openjdk-1.6.0.41-1.13.13.1.el6_8.x86_64 tzdata-java-2016j1.el6.noarch java-1.7.0-openjdk-1.7.0.131-2.6.9.0.el6_8.x86_64 --nodeps
```
上传jdk并解压然后配置环境变量
```
所有软件的安装路径 
mkdir -p /opt/lagou/servers

所有软件压缩包的存放路径 
mkdir -p /opt/lagou/software
```

```
scp ~/Downloads/jdk-8u231-linux-x64.tar.gz root@192.168.80.121:/opt/lagou/software
scp ~/Downloads/jdk-8u231-linux-x64.tar.gz root@192.168.80.122:/opt/lagou/software
scp ~/Downloads/jdk-8u231-linux-x64.tar.gz root@192.168.80.123:/opt/lagou/software
```
解压
```
tar -zxvf /opt/lagou/software/jdk-8u231-linux-x64.tar.gz -C ../servers
```
配置环境变量 vi /etc/proﬁle


```
export JAVA_HOME=/opt/lagou/servers/jdk1.8.0_231
export PATH=:$JAVA_HOME/bin:$PATH

# 修改完成后 source /etc/proﬁle 使配置⽣效
```
#### 安装rz上传⼯具 

```
yum -y install lrzsz rz 命令上传
```


## 参考文档
- [VMWare Fusion12下设置虚拟机的固定IP](https://zhuanlan.zhihu.com/p/352193144)
- [VMware Fusion NAT 无法连网解决方案](https://wsgzao.github.io/post/vmware-fusion/)
- [mac os下 vmware Fusion Linux虚拟机配置静态ip无法上网问题](https://www.cnblogs.com/ryxiong-blog/p/14543598.html)
- [如何修改 VMware Fusion 中的虚机网络 IP 地址段](https://sysin.org/article/change-vmware-fusion-networking/)