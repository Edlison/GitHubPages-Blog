---
title: Linux下的ssh密钥
catagories: 
- Notebook
tags: 
- linux
- ssh
---

# 创建SSH Key

## 生成

创建一对公钥与私钥

```sh
ssh-keygen -C "description"
```

`-C`可以对Key进行描述，描述内容将在公钥(.pub)的最后显示.

- 输入Key的名称
- 输入Key的密码（如果不需要，可以不输入）

## 复制公钥

```sh
ssh-copy-id user@hostname
```

`-i`指定公钥文件

## 登陆

```sh
ssh -i privateKey user@hostname
```

`-i`指定私钥文件

# Client

## 管理多个私钥

### 方法一

修改`~/.ssh/config`

```
Host host_alias
HostName host
Port 22
User ubuntu
IdentityFile id_rsa_common
```

有多个私钥只需要追加在后面即可。



通过配置后可以直接

```sh
ssh host_alias
```

进行登录

### 方法二

```sh
#ssh-agent
# 不输可能出错？
# 添加私钥
ssh-add id_rsa_common

# 查看私钥列表
ssh-add -l
# 清空私钥
ssh-add -D
```

# Server

## 管理多个公钥

只要将公钥中的内容直接复制到`authorized_keys`文件中即可。

## SSH配置

开启鉴权

```sh
RSAAuthentication yes
PubkeyAuthentication yes

#PermitRootLogin no
#PasswordAuthentication yes

```

## Github

将公钥直接复制到用户下的SSH中

```sh
ssh -T git@github.com
```

使用这种测试时必须`ssh-add`，否则它只会查找目录下默认的私钥名，如果自定义了名字则找不到。并且如果使用的是第一种添加私钥的方法也不行，因为此时请求的是git账号。

----

Reference

https://www.cnblogs.com/yanglang/p/9563496.html

https://blog.csdn.net/zyaiwmy/article/details/54313283

https://www.jianshu.com/p/fe215c52c534