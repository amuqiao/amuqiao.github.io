---
title: HDFS常用shell命令
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-03 14:27:00
img:
password:
summary: HSFS常用命令
tags:
- hdfs
categories:
- 大数据

---



> HSFS常用命令

### 基本语法
```
bin/hadoop fs 具体命令
OR
bin/hdfs dfs 具体命令
```
### 命令大全

```
[root@linux121 ~]# hdfs dfs
Usage: hadoop fs [generic options]
	[-appendToFile <localsrc> ... <dst>]
	[-cat [-ignoreCrc] <src> ...]
	[-checksum <src> ...]
	[-chgrp [-R] GROUP PATH...]
	[-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
	[-chown [-R] [OWNER][:[GROUP]] PATH...]
	[-copyFromLocal [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	[-copyToLocal [-f] [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
	[-count [-q] [-h] [-v] [-t [<storage type>]] [-u] [-x] <path> ...]
	[-cp [-f] [-p | -p[topax]] [-d] <src> ... <dst>]
	[-createSnapshot <snapshotDir> [<snapshotName>]]
	[-deleteSnapshot <snapshotDir> <snapshotName>]
	[-df [-h] [<path> ...]]
	[-du [-s] [-h] [-x] <path> ...]
	[-expunge]
	[-find <path> ... <expression> ...]
	[-get [-f] [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
	[-getfacl [-R] <path>]
	[-getfattr [-R] {-n name | -d} [-e en] <path>]
	[-getmerge [-nl] [-skip-empty-file] <src> <localdst>]
	[-help [cmd ...]]
	[-ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [<path> ...]]
	[-mkdir [-p] <path> ...]
	[-moveFromLocal <localsrc> ... <dst>]
	[-moveToLocal <src> <localdst>]
	[-mv <src> ... <dst>]
	[-put [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	[-renameSnapshot <snapshotDir> <oldName> <newName>]
	[-rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...]
	[-rmdir [--ignore-fail-on-non-empty] <dir> ...]
	[-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
	[-setfattr {-n name [-v value] | -x name} <path>]
	[-setrep [-R] [-w] <rep> <path> ...]
	[-stat [format] <path> ...]
	[-tail [-f] <file>]
	[-test -[defsz] <path>]
	[-text [-ignoreCrc] <src> ...]
	[-touchz <path> ...]
	[-truncate [-w] <length> <path> ...]
	[-usage [cmd ...]]

Generic options supported are:
-conf <configuration file>        specify an application configuration file
-D <property=value>               define a value for a given property
-fs <file:///|hdfs://namenode:port> specify default filesystem URL to use, overrides 'fs.defaultFS' property from configurations.
-jt <local|resourcemanager:port>  specify a ResourceManager
-files <file1,...>                specify a comma-separated list of files to be copied to the map reduce cluster
-libjars <jar1,...>               specify a comma-separated list of jar files to be included in the classpath
-archives <archive1,...>          specify a comma-separated list of archives to be unarchived on the compute machines

The general command line syntax is:
command [genericOptions] [commandOptions]
```



### HDFS命令演示
1. 启动Hadoop集群（方便后续的测试）

```
[root@linux121 hadoop-2.9.2]$ sbin/start-dfs.sh 
[root@linux123 hadoop-2.9.2]$ sbin/start-yarn.sh
```
2. -help：输出这个命令参数


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -help rm
```
3. -ls: 显示目录信息


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -ls /
```
4. -mkdir：在HDFS上创建目录

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -mkdir -p /lagou/bigdata
```
5. -moveFromLocal：从本地剪切粘贴到HDFS


```
[root@linux121 hadoop-2.9.2]$ touch hadoop.txt
[root@linux121 hadoop-2.9.2]$ hadoop fs -moveFromLocal ./hadoop.txt /lagou/bigdata

```
6. -appendToFile：追加一个文件到已经存在的文件末尾

```
[root@linux121 hadoop-2.9.2]$ touch hdfs.txt 
[root@linux121 hadoop-2.9.2]$ vi hdfs.txt

输入
namenode datanode block replication

[root@linux121 hadoop-2.9.2]$ hadoop fs -appendToFile hdfs.txt /lagou/bigdata/hadoop.txt
```

7. -cat：显示文件内容 

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -cat /lagou/bigdata/hadoop.txt
```
8. -chgrp 、-chmod、-chown：Linux文件系统中的用法一样，修改文件所属权限


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -chmod 666 /lagou/bigdata/hadoop.txt

[root@linux121 hadoop-2.9.2]$ hadoop fs -chown root:root /lagou/bigdata/hadoop.txt
```
9. -copyFromLocal：从本地文件系统中拷贝文件到HDFS路径去


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -copyFromLocal README.txt /
```

10. -copyToLocal：从HDFS拷贝到本地

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -copyToLocal /lagou/bigdata/hadoop.txt ./
```
11. -cp ：从HDFS的一个路径拷贝到HDFS的另一个路径

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -cp /lagou/bigdata/hadoop.txt /hdfs.txt
```
12. -mv：在HDFS目录中移动文件

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -mv /hdfs.txt /lagou/bigdata/
```
13. -get：等同于copyToLocal，就是从HDFS下载文件到本地


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -get /lagou/bigdata/hadoop.txt ./
```
14. -put：等同于copyFromLocal

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -mkdir -p /user/root/test/ 

#本地文件系统创建yarn.txt 
[root@linux121 hadoop-2.9.2]$ vim yarn.txt resourcemanager nodemanager

[root@linux121 hadoop-2.9.2]$ hadoop fs -put ./yarn.txt /user/root/test/
```
16. -tail：显示一个文件的末尾


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -tail /user/root/test/yarn.txt
```


17. -rm：删除文件或文件夹


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -rm /user/root/test/yarn.txt
```
18. -rmdir：删除空目录 

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -mkdir /test
 [root@linux121 hadoop-2.9.2]$ hadoop fs -rmdir /test
```


 19. -du统计文件夹的大小信息


```
[root@linux121 hadoop-2.9.2]$ hadoop fs -du -s -h /user/root/test
[root@linux121 hadoop-2.9.2]$ hadoop fs -du -h /user/root/test
```
20. -setrep：设置HDFS中文件的副本数量

```
[root@linux121 hadoop-2.9.2]$ hadoop fs -setrep 10 /lagou/bigdata/hadoop.txt
```