# MacOS安装Anaconda

## conda配置文件

位置  
`~/.condarc`

配置zsh环境变量  
`export PATH=/Applications/anaconda3/bin:$PATH`


## conda镜像源

添加镜像指令(亦可以直接在配置文件中添加)  
`conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/`  
`conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge`  
`conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/`  
 
设置搜索时显示通道地址  
`conda config --set show_channel_urls yes`



## shell虚拟环境

**取消命令行前出现的(base)**

- 每次在命令行通过`conda deactivate`退出base环境回到系统自动的环境

- `conda config --set auto_activate_base false`



## 安装TensorFlow环境

新建一个tf环境  
`conda create -n tensorflow python=3.7`

进入tf环境
`conda activate tensorflow`

pip安装tf  
`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple tensorflow`



## 卸载

安装clean包  
`conda install anaconda-clean`

文件进行删除  
`anaconda-clean`

删除Anaconda主文件包(默认路径)
`/opt/anaconda3`


