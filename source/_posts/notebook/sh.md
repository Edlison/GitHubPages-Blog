---
title: 自定义sh脚本
catagories: 
- Notebook
tags:
- shell
- zsh
---

# 变量

```sh
VAL="this is val"

LIST=(
This
is
list
)

```

使用`$VAL`或`${VAL}`获取

# 流程控制

```sh
if [ $VAL == "sz" ]
then
  ...
elif [ condition ]
then
  ...
else
  ...
fi
```



**注意**

1. []中的判断语句必须与[]间由空格隔开
2. then必须换行

## 配置文件

Server.sh

```sh
# Banner
echo "
ShenZhen: sz
V2Ray: v2ray
T40: t40(need vpn-ncepu)
"

# All servers name
SERVER_NAME=(
sz
v2ray
t40
)

# Configuration
SHENZHEN_HOST="119.23.107.61"
SHENZHEN_PORT="22"
V2RAY_HOST="47.240.166.68"
V2RAY_PORT="22"
T40_HOST="127.0.0.1"
T40_PORT="6002"

# SSH Server
if [ $1 == "sz" ]
then
  ssh root@${SHENZHEN_HOST} -p${SHENZHEN_PORT}
elif [ $1 == 'v2ray' ]
then
  ssh root@${V2RAY_HOST} -p${V2RAY_PORT}
elif [ $1 == 't40' ]
then
  ssh t40@${T40_HOST} -p${T40_PORT}
else
  echo "error: no this server!"
fi
```

Vpn.sh

```sh
# Banner
echo "
NCEPU VPN
port:6000 4*V100
port:6001 2*2080Ti
port:6002 4*T4
"

# Configueration
FRP_DIR="/Users/edlison/ExecApp/Frp/frpc"
FRP_CFG="/Users/edlison/ExecApp/Frp/cfg/frpc-ncepu.ini"

# Run Frp
${FRP_DIR} -c${FRP_CFG}
```

