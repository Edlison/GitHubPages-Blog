# 使用Docker配置Anaconda环境运行Jupyter

## 配置Anaconda环境

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



*docker-compose配置出问题？*
```yml
version: '3'

services:
  notebook:
    image: continuumio/anaconda3
    restart: "no"
    volumes:
      - ~/notebook:/notebook
      - ~/docker/config/jupyter:/root/.jupyter
    ports:
      - "11000:80"
```