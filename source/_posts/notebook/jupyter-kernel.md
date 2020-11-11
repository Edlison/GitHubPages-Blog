# 对Jupyter添加Kernel

## 对虚拟环境的操作

进入想要添加到Jupyter的Python虚拟环境

`conda activate pytorch`

安装`ipykernel`

`conda install ipykernel`

在虚拟环境中执行以下以添加Kernel到Jupyter

`python -m ipykernel install --name kernelname`

## 对Jupyter的操作

查看存在的Kernel

`jupyter kernelspec list`

删除Kernel

`jupyter kernelspec remove kernelname`

