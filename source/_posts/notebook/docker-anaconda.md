# CentOS下配置Anaconda环境运行Jupyter

## 配置Docker环境

运行docker
`$ service docker start`

```bash
Management Commands:
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  volume      Manage volumes

Commands:
  attach      Attach to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```



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

- 配置文件挂载
`-v /root/docker/config/anaconda3/jupyter_notebook_config.json:/root/.jupyter/jupyter_notebook_config.json`

*Example*  
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