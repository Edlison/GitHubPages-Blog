# Docker-GitLab

## 注意

若容器端口号设置为80，如果你这时修改external_url地址为http://ip:8080.  
那GitLab肯定访问不了，因为已经将内部的端口号修改为8080端口了，而映射的是容器的80端口。

## 故一定要将容器内部端口号与宿主机端口号映射一致！！！
external_utl 'http://119.23.107.61:13000'  
ports:
      - "13000:13000"
```yml
version: '3'

services:
  gitlab:
    image: gitlab/gitlab-ce
    restart: always
    environment:
      TZ: "Asia/Shanghai"
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://119.23.107.61:13000' // 好像需要手动改
    volumes:
      - ~/docker_gitlab/config:/etc/gitlab 
      - ~/docker_gitlab/logs:/var/log/gitlab 
      - ~/docker_gitlab/data:/var/opt/gitlab
    ports:
      - "13000:13000"
      - "13443:443"
      - "13022:22"
```