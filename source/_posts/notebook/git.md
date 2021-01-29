---
title: Git日常使用笔记
categories:
- Notebook
tags:
- git
---

![img](https://upload-images.jianshu.io/upload_images/8048507-9ea56a58f75ebc00.png?imageMogr2/auto-orient/strip|imageView2/2/w/617/format/webp)

## reset
`git reset [--soft | --mixed | --hard] [HEAD]`

`--soft`  
修改版本库，保留暂存区，保留工作区。头指针全部重置到指定版本，也会重置暂存区，并且会将工作区代码也回退到这个版本。

`--hard`  
修改版本库，修改暂存区，修改工作区。头指针全部重置到指定版本，且将这次提交之后的所有变更都移动到暂存区。

Tips:  
使用`git reset --hard HEAD`移除当前暂存区及工作区所有的修改。


## tag
给当前commit打标签。
`git tag v1.0`

删除标签
`git tag -d v1.0`

显示标签修改内容
`git show v1.0`

## log
查看最近提交的文件差异，最近两篇。  
`git log -p -2`

## clean
用于删除Untracked file, 常与`git reset --hard`一起使用，完全回复到指定commit。  
`git clean -df`d为文件夹，f为文件。

