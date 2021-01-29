---
title: Python中的*arg与**kwarg
categories:
- Notebook
tags:
- python
---


## 参数的规则

### 参数的顺序1：
位置参数 - *arg - **kwarg

*arg会把多出来的位置参数转化为`tuple`
**kwarg会把关键字参数转化为`dict`

### 参数的顺序2：
位置参数 - * - 关键字...

关键字亦看作位置参数，如果**不传则会报错**。


## 传参时的技巧

获取参数时：
*可以看作是将参数打包成`tuple`
**可以看作是将参数打包成`dict`

传参时：
可以将*tuple解压包并传入
可以将**dict解压包并传入




----
Reference: https://www.jianshu.com/p/e0d4705e8293
