---
title: Spark - 部署
categories:
- Notebook
tags:
- spark
---

# 安装

从腾讯云镜像安装(这是内网地址)

`wget http://mirrors.tencentyun.com/apache/xxx`

解压到`~/applications`

创建软连接，方便管理不同版本。

`ln -s spark-3.0.1-bin-hadoop2.7 spark`

`export SPARK_HOME=/home/ubuntu/applications/spark`

以后只要将软连接的指向改动到新版本，不需要改动环境变量。

# 启动

## 安装JDK

`sudo apt install openjdk-11-jdk`

## 使用Scala Shell启动Spark

`spark-shell`

## 简单的例子

```scala
var file = sc.textFile("/home/ubuntu/documents/xxx.txt")
// 打印行数
file.count()
// 打印第一行
file.first()
// 过滤打印
file.filter(line => line.contains("spark")).count()
```

## 启动节点

### 主节点

`cd $SPARK_HOME/sbin`

`./start-master.sh`

通过`ip:8080`可以web访问

获取Master节点的Url

`spark://localhost.localdomain:7077`

### 从节点

`./start-slave.sh spark://localhost.localdomain:7077`

可以通过`jps`（显示当前的Java进程）查看启动的服务

Web端可以看到有了Workers

### 用Shell连接Master

```sh
MASTER=spark://localhost.localdomain:7077 spark-shell
```

Web端可以看到RunningApplications有了Shell

### 停止服务

`./stop-all.sh`

`jps`查看

# 集群部署

## 配置文件

主节点配置环境变量`conf/spark-env.sh`

主节点配置从节点的主机名`conf/slaves`, 此时主机名要写入hosts中

将spark整体文件拷贝到所有从节点，路径与主节点一致

配置**主节点到从节点**的SSH Key无密码登录

启动集群`sbin/start-all.sh`







