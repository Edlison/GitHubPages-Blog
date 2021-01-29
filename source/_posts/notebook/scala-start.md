---
title: Scala - Start
categories:
- Notebook
tags:
- scala
---

# 安装

从官网下载二进制文件

`mv scala /opt/scala`

打开Idea指定SDK之后就可以创建Scala项目了

# 开始

在Idea中新建一个Scala项目

创建一个`scala object`

```scala
object Test {
  def main(args: Array[String]): Unit = {  
    print("hello world!")
  }
}
```

可以直接运行

也可以使用命令行

```sh
scala Test.scala
```

# 注意

Scala中的object相当于静态类，main只能处于object中。

class只有在被实例化的时候才会被加载。



----

Reference

https://www.jianshu.com/p/e0fc0ab7a9d2