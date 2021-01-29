---
title: Fluent-Python-PartI-Prologue
catagories: 
- Fluent-Python
tags: 
- python
---

# 开始Fluent-Python

前言：

Here's the plan: when someone uses a feature you don't understand, simply shoot them.

# 1-The Python Data Model

魔术方法也叫做Dunder方法，一般under-under-getitem-under-under也可以叫做dunder-getitem.

Example 1-1

我们可以通过重写`__len__`方法与`__getitem__`方法实现对象的获取书长度，与获取数据。

特殊方法的调用时隐式的，例如`for i in x:`就隐式的调用了iter(x).

代码中不应该有很多对特殊方法的直接调用，除非你在进行元编程。



Example 1-2

`__repr__`与`__str__`方法类似，但是在打印一个实例的信息时会调用`__repr`方法。如果没有重写该方法console会显示类似`<Vector object at 0x10e100070>`的信息。

`__str__`由str()函数调用，强烈建议重写`__repr__`如果没有`__str__`则会调用`__repr__`.

`bool()`调用`__bool__`的结果，如果没有实现`__bool__`那么会调用`__len__`如果长度为0则返回`False`.





