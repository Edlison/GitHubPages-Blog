---
title: Hadoop - mapreduce
categories:
- Notebook
tags:
- hadoop
- mapreduce
---

# 简介

MapReduce就是一个分布式的计算框架，用于并行处理数据。

# 步骤

分为两步Map和Reduce

- Map阶段：处理输入数据，从HDFS读取数据，映射器处理该数据，并创建中间数据。
- Reduce阶段：Shuffle+Reduce处理映射器中的数据，产生输出，存储在HDFS中。

# 实例

1. 写MapReduce程序

```sh
ProcessUnits.java
```

2. 编译该Java类

```sh
javac -classpath xxx -d folder xxx.java
```

3. 将需要处理的数据存入HDFS

```sh
hadoop fs -put xxx
```

4. 运行MapReduce程序

```sh
hadoop jar xxx input_folder output_folder
```

5. `hadoop`可以操作MapReduce等其它工作及设置

```sh
hadoop cmd
```

