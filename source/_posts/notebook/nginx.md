# Nginx

检测配置文件
`nginx -t`

重启服务
`nginx -s reload`

若重启报错 先运行  
`nginx -c /etc/nginx/nginx.conf`

SSL config
```conf
    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  api.gfgf.tech;
        root         /usr/share/nginx/html;

	    return 301 https://$server_name$request_uri;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
                proxy_pass http://127.0.0.1:21000;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  api.gfgf.tech;
        root         /usr/share/nginx/html;

        ssl_certificate "/root/.acme.sh/api.gfgf.tech/api.gfgf.tech.cer";
        ssl_certificate_key "/root/.acme.sh/api.gfgf.tech/api.gfgf.tech.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
		proxy_pass http://localhost:21000;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```