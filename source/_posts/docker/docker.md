# Docker & Docker-Compose


## 运行docker
`$ service docker start`



## 运行容器
`docker run -i -t -p 11000:8888 continuumio/anaconda3 /bin/bash`
`-d`detach直接后台运行

```
-i: 是  以交互模式运行容器，通常与 -t 同时使用；
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-p: 指定端口映射，格式为：主机(宿主)端口:容器端口
```

**退出容器并保存在后台**
`Control + P + Q`

**进入容器**
`docker exec -it name /bin/bash`
此时`exit`不会关闭容器

**查看运行的容器**
`docker ps -a`



## 映射或挂载(重要)
`宿主机:镜像`

**默认挂载路径**  
`/var/lib/docker/volumes`



## 镜像相关

**生成镜像**  
`docker commit -a "username" -m "msg" container_name new_image_name`

**上传镜像**  
将打包好的新镜像上传到 Docker Hub：
 
首先我们需要将这个新镜像打上 tag，方便在公共服务器进行上传：

`docker tag new_anaconda_xgboost:latest nimendavid/machine_learning:v0.1`

其中:

`new_anaconda_xgboost:latest`
(本地镜像名称:tag)

可以通过 `docker image ls` 查看
`nimendavid/machine_learning:v0.1`

格式是(dockerhub用户名/仓库名:tag)  ，需要自己有一个dockerhub账号，v0.1就是自定义的版本号码

然后记得登录在服务器上dockerhub，否则推送会报错：

`docker login`