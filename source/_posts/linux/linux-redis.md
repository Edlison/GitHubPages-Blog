---
title: Linux安装Redis
catagories: 
- Notebook
tags: 
- linux
- redis
---

# Linux版安装流程

1. 获取最新的版本

https://redis.io/download

2. 下载，解压，编译

```sh
$ wget https://download.redis.io/releases/redis-6.0.10.tar.gz
$ tar xzf redis-6.0.10.tar.gz
$ cd redis-6.0.10
$ make
```

3. 测试

```sh
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```



# 注意

1. Redis 6需要gcc的版本为5.3以上

- 查看gcc版本

```sh
gcc -v
```

- 升级

```sh
#升级到 5.3及以上版本
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils

scl enable devtoolset-9 bash
```

scl命令启用只是临时的，推出xshell或者重启就会恢复到原来的gcc版本。
如果要长期生效的话，执行如下：

```sh
echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile
```

2. 遇到fatal error

```sh
make MALLOC=libc
```

----

Reference

https://blog.csdn.net/hello_cmy/article/details/106062327

https://blog.csdn.net/libra_ts/article/details/71195128