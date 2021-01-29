---
title: 从0开始配置Ubuntu系统
catagories: 
- Notebook
tags: 
- cloud
- vps
---

# 创建Ubuntu系统镜像

Ubuntu默认的账号是ubuntu，腾讯云创建的密码也是ubuntu对应的密码。

默认情况下，不能通过root使用密码远程登录。

执行一下操作获取root用户远程密码登录的权限。

- 设置root密码`sudo passwd root`
- 设置ssh配置文件`sudo vim /etc/ssh/sshd_config`修改`PermitRootLogin yes`
- 重启服务`sudo service ssh restart`

# Ubuntu用户

用户默认使用Ubuntu，在执行特殊操作时使用`sudo`指令。

需要新建`.bashrc`来存储用户的`bash`配置文件。





----

Reference

https://cloud.tencent.com/developer/article/1405735