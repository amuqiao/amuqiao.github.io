---
title: HDFS读写数据解析
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-04 17:22:31
img:
password:
summary: 介绍hdfs读写原理
tags:
- hdfs
- hadoop
categories:
- 大数据
---


### HDFS读数据流程
![读数据流程](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/s76ija.png)
1. 客户端通过Distributed FileSystem向NameNode请求下载文件，NameNode通过查询元数据， 找到文件块所在的DataNode地址。
2. 挑选一台DataNode（就近原则，然后随机）服务器，请求读取数据。
3. DataNode开始传输数据给客户端（从磁盘里面读取数据输入流，以Packet为单位来做校验）。
4. 客户端以Packet为单位接收，先在本地缓存，然后写入目标文件。

### HDFS写数据流程

![写数据流程](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/vB0RtP.png)
1. 客户端通过Distributed FileSystem模块向NameNode请求上传文件，NameNode检查目标文件 是否已存在，父目录是否存在。
2. NameNode返回是否可以上传。
3. 客户端请求第一个 Block上传到哪几个DataNode服务器上。
4. NameNode返回3个DataNode节点，分别为dn1、dn2、dn3。
5. 客户端通过FSDataOutputStream模块请求dn1上传数据，dn1收到请求会继续调用dn2，然后 dn2调用dn3，将这个通信管道建立完成。
6. dn1、dn2、dn3逐级应答客户端。
7. 客户端开始往dn1上传第一个Block（先从磁盘读取数据放到一个本地内存缓存），以Packet为单 位，dn1收到一个Packet就会传给dn2，dn2传给dn3；dn1每传一个packet会放入一个确认队列 等待确认。
8. 当一个Block传输完成之后，客户端再次请求NameNode上传第二个Block的服务器。（重复执行 3-7步）。
9. 

### 演示从本地文件系统上传文件到hdfs

```java
@Test
public void uploadPackage() throws IOException {
    //1. 准备读取本地文件的输入流
    FileInputStream fileInputStream = new FileInputStream(new File("/Users/wangqiao/Downloads/test.txt"));

    //2. 准备好写出数据到hdfs的输出流
    FSDataOutputStream fsDataOutputStream = fs.create(new Path("/api_test/test.txt"), new Progressable() {
        public void progress() {
            // 这个progress方法就是每传输64KB（packet）就会执行一次
            // 如果有数据要传递，建立通道时会执行一次
            // 所以当传输 0kb的文件时，不会打印&
            // 传递小于64kb的文件时打印2个&
            // 传递小于128kb的文件时会打印3个&
            System.out.println("&");
        }
    });

    //3. 实现流拷贝 默认关闭流选项是true，所以会自动关闭
    IOUtils.copyBytes(fileInputStream, fsDataOutputStream, conf);
    //4. 关闭流 可以再次关闭也可以不关了
    IOUtils.closeStream(fsDataOutputStream);
    IOUtils.closeStream(fileInputStream);
}
```