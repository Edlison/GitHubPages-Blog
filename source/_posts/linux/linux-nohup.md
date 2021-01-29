---
title: Linux中的nohup
categories:
- Notebook
tags:
- linux
---

# nohup输出到指定文件

输出到默认文件`nohup.out`

```sh
nohup python train.py&
```

输出到指定文件

```sh
nohup python train.py&> train.out&
```

# 实时监控输出内容

```sh
tail -f nohup.out
```



# 查看程序进程

`ps`显示当前进程状态，及其启动时的命令行参数

```sh
ps -ef | grep cmd
```

