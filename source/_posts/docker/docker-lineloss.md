# Docker 部署 Lineloss

## 1 拉取redhat镜像
`docker pull yjjy0921/redhat7.2`



## 2 进入镜像换yum源
- rpm安装wget(docker cp host.path cotainer:container.path)
- wget下载yum相关包
- rpm安装下载的相关包

**报错：**
python-urlgrabber >= 3.10-8 is needed by yum-3.4.3-167.el7.centos.noarch  
rpm >= 0:4.11.3-22 is needed by yum-3.4.3-167.el7.centos.noarch

查询存在的python-urlgrabber包
`rpm -q python-urlgrabber`~~

删除python-urlgrabber包
`rpm -e python-urlgrabber`

更新rpm包
`rpm -Uvh rpm-4.11.3-43.el7.x86_64.rpm --nodeps`



## 3 创建aliyun.repo
`cd /etc/yum.repo.d/`
`vim aliyun.repo`

```
[base]
name=CentOS-$releasever - Base
baseurl=http://mirrors.aliyun.com/centos/7.8.2003/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/7.8.2003/os/x86_64/RPM-GPG-KEY-CentOS-7


#released updates
[updates]
name=CentOS-$releasever - Updates
baseurl=http://mirrors.aliyun.com/centos/7.8.2003/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/7.8.2003/os/x86_64/RPM-GPG-KEY-CentOS-7


[extras]
name=CentOS-$releasever - Extras
baseurl=http://mirrors.aliyun.com/centos/7.8.2003/extras//$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/7.8.2003/os/x86_64/RPM-GPG-KEY-CentOS-7

[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://mirrors.aliyun.com/centos/7.8.2003/centosplus//$basearch/
gpgcheck=1
enabled=0
```

清除redhat.repo
`mv redhat.repo redhat.temp`

检验yum配置成功
`yum update -y --skip-broken`



## 4 安装mariadb

安装
`yum install -y mysql`

**注意**
docker无法使用systemctl指令使mysql后台运行

查看容器配置文件
`docker inspect containerID`

*修改已经运行的容器配置文件*
`/usr/lib/docker...`

运行镜像时需要
`docker run --privileged --name=lineloss -d -it edlison/lineloss /usr/sbin/init`

`--privileged`赋予额外权限
`/usr/sbin/init`将cmd设置为这个 dbus服务就会运行

`systemctl start mariadb` 启动服务
`systemctl enable mariadb` 设置开机启动
`systemctl restart mariadb` 重新启动
`systemctl stop mariadb.service` 停止MariaDB

初始化数据库
`mysql_secure_installation`

设置密码
`mysqladmin -u root password 'newpassword'`

导入数据
`create database lineloss;`
`use lineloss;`
`source /home/xs_project/sql/lineloss.sql`



## 5 Java环境

安装Java
`yum install java`



## 6 Python环境

安装python3
`yum install python3`

安装依赖
`pip3 install Flask`
`pip3 install flask_sqlalchemy`
`pip3 install pymysql`
`pip3 install numpy`
`pip3 install sklearn`
`pip3 install jieba`
`pip3 install pandas`
`pip3 install statsmodels`



## 7 运行测试

运行镜像
`docker run --privileged --name=lineloss -d -p 18084:18084 -it edlison/lineloss /usr/sbin/init`

Java
`cd /home/xs_project/java`
`java -jar RuoYi.jar`

Python
`cd /home/xs_project/python/LineLoss`
`python3 service.py`



## 8 Dockerfile

只是构建镜像？？？不便于执行镜像内程序？？？



## 9 其他
## 复制文件

容器内新建文件夹
`mkdir /home/xs_project/java`
`mkdir /home/xs_project/python`
`mkdir /home/xs_project/sql`

将文件复制到容器内
`cp RuoYi.jar containerID:/home/xs_project/java`
`cp LineLoss.zip containerID:/home/xs_project/python`
`cp lineloss.sql containerID:/home/xs_project/sql`

## 容器提交成镜像

`docker commit -a "userid" -m "comment" containerID imageName:tag`

## 镜像上传仓库

`docker push userName/imageName`

**注意**

tag命令修改为规范的镜像名
`docker tag lineloss:latest edlison/lineloss`

push到用户仓库
`docker push edlison/lineloss`


## 镜像导出导入

导出镜像
`docker save -o lineloss.tar edlison/lineloss`或
`docker save edlison/lineloss>/home/lineloss.tar`

导入镜像
`docker load<lineloss.tar`

