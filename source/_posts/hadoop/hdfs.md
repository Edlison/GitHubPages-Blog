---
title: Hadoop - hdfs
categories:
- Notebook
tags:
- hadoop
- hdfs
---

# 架构

HDFS是Master和Slave结构，分为NameNode, Secondary NameNode, DataNode.

- NameNode: 在Hadoop中只有一个，管理HDF的名称空间，数据块映射信息等。
- Secondary: 辅助NameNode, 分担NameNode的工作，可以辅助恢复NameNode.
- DataNode: Slave节点，实际存储数据，执行数据块的读写并汇报存储信息给NameNode。

# 操作

## 将数据插入到HDFS

`hadoop fs -mkdir`

`hadoop fs -put`

...

## 从HDFS获取数据到本地文件系统

`hadoop fs -get`

...

## 命令参考

https://www.yiibai.com/hadoop/hadoop_command_reference.html

