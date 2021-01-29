---
title: 查看服务器各种属性
catagories: 
- Notebook
tags: 
- linux
---

# CPU属性

```shell
user@server:~$ lscpu

Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              72
On-line CPU(s) list: 0-71
Thread(s) per core:  2
Core(s) per socket:  18
Socket(s):           2
NUMA node(s):        2
Vendor ID:           GenuineIntel
CPU family:          6
Model:               85
Model name:          Intel(R) Xeon(R) Gold 5220 CPU @ 2.20GHz
Stepping:            7
CPU MHz:             999.916
CPU max MHz:         3900.0000
CPU min MHz:         1000.0000
BogoMIPS:            4400.00
Virtualization:      VT-x
L1d cache:           32K
L1i cache:           32K
L2 cache:            1024K
L3 cache:            25344K
NUMA node0 CPU(s):   0-17,36-53
NUMA node1 CPU(s):   18-35,54-71
```



# 查看内存

-g是指以GB为单位，-m是以MB为单位。

`free -g`

详细信息

`cat /proc/meminfo`

# 查看硬盘

**硬盘和分区分布**

`lsblk`

**硬盘和分区详细**

`fdisk -l`

**硬盘空间剩余**

`df -h`

**硬盘空间使用情况**

当前目录的大小`du -sh`

指定文件的大小`du filename`

目录所占空间情况`du -h foldername`

`-h`提高可读性

# 查看显卡

`nvidia-smi`

# 根据PID查看进程详细信息



`ls -l /proc/${pid}`



`ps -ef | grep ${pid}`