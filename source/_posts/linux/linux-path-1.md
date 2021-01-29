---
title: Linux Path 环境变量设置
categories:
- Notebook
tags:
- linux
---

# 新认识

**显示环境变量**

`echo $PATH`

可以用`echo`查看任何设置好的变量

`echo $JAVA_HOME`

**添加环境变量**

`export PATH=$PATH:pwd`

`$PATH:`其实就是在PATH后面继续添加新路径，用`:`连接。

**查找环境变量中的文件**

通过`which`查找符合的在环境变量中的文件。

例：

已知有环境变量中有java，需要查找java所在目录以设置`JAVA_HOME`变量。

`which java`查找到java执行地址，通过`ls -l pwd`找到该地址指向的地址，依次一直查找下去，找到java所在的真正地址。

`export JAVA_HOME=pwd`

