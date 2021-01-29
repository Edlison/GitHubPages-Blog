---
title: Linux硬连接和软连接
categories:
- Notebook
tags:
- linux
---

# 介绍

Linux下的快捷方式称作硬连接(hard link)或软连接(symbolic link).

# 例子

创建一个文件`file.txt`

`echo 'hello world.' > file.txt`

创建硬连接

`ln file.txt hardlink`

创建软连接

`ln -s file.txt softlink`

查看文件属性

`ls -li`

```sh
262781 -rw-rw-r-- 2 ubuntu ubuntu 13 Jan 28 15:18 file.txt
262781 -rw-rw-r-- 2 ubuntu ubuntu 13 Jan 28 15:18 hardlink
262991 lrwxrwxrwx 1 ubuntu ubuntu  8 Jan 28 15:19 softlink -> file.txt
```



可以看到`softlink`指向源文件

而`hardlink`与源文件共享一个`inode`

# 总结

删除源文件，硬连接不受影响，仍可读取源文件的内容；软连接无法访问源文件的内容。

硬连接类似于一个文件的备份。

硬连接限制多，无法给文件夹创建硬连接。

# 应用

控制同一个软件的不同版本。



----

Reference

https://cloud.tencent.com/developer/article/1683660