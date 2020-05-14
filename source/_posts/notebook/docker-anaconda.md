# 使用Docker配置Anaconda环境运行Jupyter

## 配置Anaconda环境

运行容器
`docker run -i -t -p 11000:8888 continuumio/anaconda3 /bin/bash`
`-d`detach直接后台运行

```
-i: 是  以交互模式运行容器，通常与 -t 同时使用；
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-p: 指定端口映射，格式为：主机(宿主)端口:容器端口
```

退出容器并保存在后台
`Control + P + Q`

进入容器
`docker exec -it name /bin/bash`
此时`exit`不会关闭容器

查看运行的容器
`docker ps -a`

**挂载**
- 文件夹挂载
`-v /root/notebook:/notebook`
(宿主机目录:镜像内挂载目录)

- *配置文件挂载*(生成的文件夹)
`-v /root/docker/config/anaconda3/jupyter_notebook_config.json:/root/.jupyter/jupyter_notebook_config.json`

**Example**  
`docker run -i -t -p 11000:8888 -v ~/notebook:/notebook -v ~/docker/config/jupyter:/root/.jupyter continuumio/anaconda3 /bin/bash`  
挂载参数要在容器前



## 配置Jupyter Notebook

`jupyter notebook --allow-root`

启动Notebook
`jupyter notebook --port 8888 --ip 0.0.0.0 --allow-root`

后台运行Notebook
`nohup jupyter notebook --port 8888 --ip 0.0.0.0 --allow-root &`

**相关配置**

配置文件
```py
c = get_config()
c.IPKernelApp.pylab = "inline"
c.NotebookApp.ip = "*"
c.NotebookAPp.open_browser = False
c.NotebookApp.password = 'sha1:5295d47ebd06:cdca9499f90b4b4616c935fdff61dda71e1e4393'
# c.NotebookApp.certfile = u'/root/.jupyter/mycert.pem'
c.NotebookApp.port= 8888
c.NotebookApp.notebook_dir = "/"
```



## Docker相关

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