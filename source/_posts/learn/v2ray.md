# V2ray

## 安装V2ray
- 安装脚本

执行安装脚本

`$ wget https://install.direct/go.sh`

运行脚本

`$ sudo bash go.sh`

- 运行V2ray

`$ sudo systemctl start v2ray`

- V2ray配置文件

`/etc/v2ray/config.json`

## TLS
- 安装脚本

执行以下命令，acme.sh 会安装到 `~/.acme.sh` 目录下。
`$ curl https://get.acme.sh | sh`

安装成功后执行 `source ~/.bashrc` 以确保脚本所设置的命令别名生效。

如果安装报错，那么可能是因为系统缺少 acme.sh 所需要的依赖项，acme.sh 的依赖项主要是 socat，我们通过以下命令来安装这些依赖项，然后重新安装一遍 acme.sh:

`$ sudo apt-get install openssl cron socat curl`

- 分发证书

使用Nginx监听80端口进行证书分发

`$ acme.sh --issue  -d tunnel.edlison.xyz  --nginx`

使用acme自带的standalone进行端口监听

`$ acme.sh --issue -d tunnel.edlieon.xyz --standalone`

Example

`$ acme.sh --issue -d tunnel.edlison.xyz --nginx --keylength ec-256`

`--keylength` 表示密钥长度，后面的值可以是 ec-256 、ec-384、2048、3072、4096、8192，带有 ec 表示生成的是 ECC 证书，没有则是 RSA 证书。在安全性上 256 位的 ECC 证书等同于 3072 位的 RSA 证书

- 更新证书

`$ acme.sh --renew -d tunnel.edlison.xyz --force --ecc`

由于 Let's Encrypt 的证书有效期只有 3 个月，因此需要 90 天至少要更新一次证书，acme.sh 脚本会每 60 天自动更新证书。也可以手动更新。`--ecc`生成的ECC证书

- 安装证书和密钥

将证书安装到V2ray目录下

```bash
$ acme.sh --installcert -d tunnel.edlison.xyz --ecc \
                          --fullchain-file /etc/v2ray/v2ray.crt \
                          --key-file /etc/v2ray/v2ray.key
```

- 配置文件

```json
{
  "inbounds": [
    {
      "port": 443, // 建议使用 443 端口
      "protocol": "vmess",    
      "settings": {
        "clients": [
          {
            "id": "23ad6b10-8d1a-40f7-8ad0-e3e35cd38297",  
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "tls", // security 要设置为 tls 才会启用 TLS
        "tlsSettings": {
          "certificates": [
            {
              "certificateFile": "/etc/v2ray/v2ray.crt", // 证书文件
              "keyFile": "/etc/v2ray/v2ray.key" // 密钥文件
            }
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

## Nginx

- Start

配置文件

`$ nginx -c /etc/nginx/nginx.conf`

启动Nginx

`$ nginx -s reload`

- config

Nginx 仅需监听80端口

```nginx
server {
        listen       80;
        listen       [::]:80;
        server_name  tunnel.edlison.xyz;

        #return  301 https://$server_name$request_uri; // 可以将流量转向https

        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```

## WebSocket + TLS + Web

这次 TLS 的配置将写入 Nginx/Caddy/Apache 配置中，由这些软件来监听 443 端口（443 比较常用，并非 443 不可），然后将流量转发到 V2Ray 的 WebSocket 所监听的内网端口（本例是 10000），V2Ray 服务器端不需要配置 TLS。

- V2ray Settings

```json
{
  "inbounds": [
    {
      "port": 10000,
      "listen":"127.0.0.1",//只监听 127.0.0.1，避免除本机外的机器探测到开放了 10000 端口
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "b831381d-6324-4d53-ad4f-8cda48b30811",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/ray"  // 路径
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

- TLS

证书生成见上文

- Nginx Setting

```nginx
server {
  listen 443 ssl;
  listen [::]:443 ssl;
  
  ssl_certificate       /etc/v2ray/v2ray.crt;
  ssl_certificate_key   /etc/v2ray/v2ray.key;
  ssl_session_timeout 1d;
  ssl_session_cache shared:MozSSL:10m;
  ssl_session_tickets off;
  
  ssl_protocols         TLSv1.2 TLSv1.3;
  ssl_ciphers           ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
  ssl_prefer_server_ciphers off;
  
  server_name           tunnel.edlison.xyz;
  location /ray { # 与 V2Ray 配置中的 path 保持一致
    if ($http_upgrade != "websocket") { # WebSocket协商失败时返回404
        return 404;
    }
    proxy_redirect off;
    proxy_pass http://127.0.0.1:10000; # 假设WebSocket监听在环回地址的10000端口上
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    # Show real IP in v2ray access.log
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
```

- Client Setting

```json
{
  "inbounds": [
    {
      "port": 1080,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth",
        "udp": false
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "mydomain.me",
            "port": 443,
            "users": [
              {
                "id": "b831381d-6324-4d53-ad4f-8cda48b30811",
                "alterId": 64
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
        "wsSettings": {
          "path": "/ray"
        }
      }
    }
  ]
}
```

- 注意事项

注意事项
V2Ray 自 4.18.1 后支持 TLS1.3，如果开启并强制 TLS1.3 请注意 v2ray 客户端版本.
较低版本的 nginx 的 location 需要写为 /ray/ 才能正常工作
如果在设置完成之后不能成功使用，可能是由于 SElinux 机制(如果你是 CentOS 7 的用户请特别留意 SElinux 这一机制)阻止了 Nginx 转发向内网的数据。如果是这样的话，在 V2Ray 的日志里不会有访问信息，在 Nginx 的日志里会出现大量的 "Permission Denied" 字段，要解决这一问题需要在终端下键入以下命令：
setsebool -P httpd_can_network_connect 1
请保持服务器和客户端的 wsSettings 严格一致，对于 V2Ray，/ray 和 /ray/ 是不一样的
较低版本的系统/浏览器可能无法完成握手. 如 Chrome 49/XP SP3, Safari 8/iOS 8.4, Safari 8/OS X 10.10 及更低的版本. 如果你的设备比较旧, 则可以通过在配置中添加较旧的 TLS 协议以完成握手.

## BBR

- 一键安装脚本

`$ wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh`

- 检测内核

`$ uname -r`

- 检测安装情况

`$ sysctl net.ipv4.tcp_available_congestion_control`