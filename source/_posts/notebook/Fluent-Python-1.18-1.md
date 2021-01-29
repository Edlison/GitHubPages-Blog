---
title: Fluent-Python-PartIII-Functions as Objects
catagories: 
- Fluent-Python
tags: 
- python
- design-pattern
---

# 5-First-Class Functions

Python中所有的函数都是first-class对象（一级对象？）

- 在运行时创建
- 赋值给数据结构中的变量或元素
- 作为参数传递给函数
- 作为函数的结果返回

## 可调用对象的7中风格

1. 用户定义方程

使用`def`语句或`lambda`表达式创建。

2. 内建函数

`len`

3. 内建方法

Dice.get

4. 方法

定义在类内部的函数

5. 类

可以调用`__new__`方法去生成实例的类，然后通过`__init__`方法来初始化它。

6. 类实例

如果一个类定义了一个`__call__`方法，那这个实例就会当作函数被调用。

7. 生成器函数

函数或方法使用`yield`关键字，当调用的时候，生成器函数返回一个生成器对象。

## User-Defined Callable Types用户定义的可调用类型

Python函数不仅是真正的对象，而且任意的Python对象也可以表现的像函数一样。只要实现`__call__`实例方法。



# 6-Design Patterns with First-Class Functions

项目实例

