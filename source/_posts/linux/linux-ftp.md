---
itle: Linux安装ftp
categories:
- Notebook
tags:
- linux
- ftp
---

# 安装步骤

1. yum安装vsftpd

`yum install vsftpd`

2. 设置开机启动

`systemctl enable vsftpd`

3. 启动FTP

`systemctl start vsftpd`

# 配置文件

所在目录

`cd /etc/vsftpd/vsftpd.conf`

常用设置

```
# 端口号
listen_port=21
# 监听IPv4 sockets
listen=YES
# 这个设置后/etc/passwd内的账号才可以登陆
local_enable=YES
# 这个设置后list中的用户只能访问自己的/home下的目录
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list
# 允许匿名登陆
anonymous_enable=YES
# 匿名登陆者的根目录
anon_root=/var/ftp
# 本地用户登陆后的根目录
local_root=/home
# 被动模式 开启后相当于服务端开启范围内端口等待客户端来连接
pasv_enable=YES
pasv_min_port=4000
pasv_max_port=4500
```

**有问题？**

- `local_root=/var/ftp`设置后用户进去还是到对应的`/home`文件夹。
- `chroot_local_user=YES`设置后用户可以跳出对应的`/home`下的用户文件夹，有安全风险。

**注意**

- 监听sockets后，允许匿名，要开启被动模式用户才可以网站匿名进入。

- `pasv`相关的内容设置后好想要`systemctl restart`。

# 详解

安装

https://cloud.tencent.com/document/product/213/10912#user

配置文件解析

https://www.cnblogs.com/zhangzhiqin/p/10170201.html