# [Linux]安装Java在多个版本中切换

## 查看yum中的Java版本
`$ yum search java-1.8`

## 安装JDK
`yum install java-1.8.0-openjdk-devel`

安装JDK包含JRE

Java Development EnvironmentKit 为 JDK
Java Runtime Environment 为 JRE

## 切换Java版本
`$ sudo update-alternatives --display java` 查看安装了几个java
`$ sudo update-alternatives --config java` 设置java的版本
`$ alternatives --config javac` 设置javac的版本

如已手动安装的可以用下面的命令去添加到列表
`$ alternatives --install /usr/bin/java java /usr/java/jdk1.8/bin/java 180000`
`$ alternatives --install /usr/bin/java javac /usr/java/jdk1.8/bin/javac 180000`
