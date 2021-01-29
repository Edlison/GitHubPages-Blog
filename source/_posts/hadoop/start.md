---
title: Hadoop - start
categories:
- Notebook
tags:
- hadoop
---

# QuikStart

## 安装Hadoop

`wget https://archive.apache.org/dist/hadoop/common/hadoop-3.2.2/hadoop-3.2.2.tar.gz`

# Hadoop操作模式

可以选择Hadoop集群以以下三个支持模式之一：

- 独立/单机模式：默认情况下，被配置在一个独立的模式中运行Java程序。
- 模拟分布式模式：这是单台机器的分布式模拟。Hadoop守护每个进程，如hdfs, yarn, MapReduce等，都将作为一个独立的Java程序运行，对开发有利。
- 完全分布式模式：这种模式时完全分布式的最小两台或多台计算机的集群。

# 在单机模式下安装Hadoop

独立/单机模式适合开发期间运行MapReduce程序，容易进行测试和调试。

**设置`JAVA_HOME`变量**

`export JAVA_HOME=pwd`

**设置Java环境变量**

`export PATH=$PATH:$JAVA_HOME/bin`

将Hadoop添加到环境变量

`export PATH=$PATH:/home/hadoop/bin`

**检验Hadoop安装**

`hadoop version`

**测试基本功能**

编译MapReduce实例，提供了若干功能。

`hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.2.jar`

使用MapReduce计算单词个数

`hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.2.jar wordcount input output`

# 模拟分布式模式安装Hadoop

在伪分布式模式下安装Hadoop

## 设置Hadoop

```shell
export HADOOP_HOME=/usr/local/hadoop 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export HADOOP_COMMON_HOME=$HADOOP_HOME 
export HADOOP_HDFS_HOME=$HADOOP_HOME 
export YARN_HOME=$HADOOP_HOME 
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin 
export HADOOP_INSTALL=$HADOOP_HOME
```

提交

`source ~/.bashrc`

## Hadoop配置  TODO 配置文件的意义

core-site.xml

```xml
<configuration>

   <property>
      <name>fs.default.name </name>
      <value> hdfs://localhost:9000 </value> 
   </property>
 
</configuration>
```

hdfs-site.xml

```xml
<configuration>

   <property>
      <name>dfs.replication</name>
      <value>1</value>
   </property>
    
   <property>
      <name>dfs.name.dir</name>
      <value>file:///home/hadoop/hadoopinfra/hdfs/namenode </value>
   </property>
    
   <property>
      <name>dfs.data.dir</name> 
      <value>file:///home/hadoop/hadoopinfra/hdfs/datanode </value> 
   </property>
       
</configuration>
```

yarn-site.xml

```xml
<configuration>
 
   <property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value> 
   </property>
  
</configuration>
```

mapred-site.xml

```xml
<configuration>
 
   <property> 
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
   </property>
   
</configuration>
```

## 验证安装  TODO 指令含义

1. 名称节点设置

`hdfs name -format`

2. 验证DFS

`start-dfs.sh`

3. 验证Yarn

`start-yarn.sh`

**注意**

1. 报错`ERROR: Attempting to operate on hdfs namenode as root`

在`start-dfs.sh`, `stop-dfs.sh`文件添加

```shell
HDFS_DATANODE_USER=root
HADOOP_SECURE_DN_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
```

在`start-yarn.sh`, `stop-yarn.sh`文件添加

```shell
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root
```

2. 报错`Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).`

```shell
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub root@Ali-Edlison-Server01
```

## 浏览器访问Hadoop  TODO 端口对应含义

访问集群信息

`http://ip:8042/`







----

References

https://www.yiibai.com/hadoop/hadoop_enviornment_setup.html

https://www.cnblogs.com/woofwoof/p/10024104.html

