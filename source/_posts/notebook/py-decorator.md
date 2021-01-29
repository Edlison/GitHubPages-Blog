---
title: Python装饰器(Decorator)
categories:
- Notebook
tags:
- python
---

## 一切皆对象
函数也可以是对象，如果不加()即视为不调用该函数，只是获取函数的对象。
```py
def hi(name='edlison')
    return 'hello' + name

greet = hi

print(greet())  # 'hi edlison'
```


## 函数中定义函数
```py
def hi(name='edlison')
    print('in hi()')

    def greet():
        return 'in greet'
    
    def hello():
        return 'in hello'
    
    print('back')
```
函数内部依次执行，函数内部的函数只能内部调用。


## 函数中返回函数
```py
def hi(name='edlison'):
    def greet()
        return 'in greet()'
    
    if name == 'edlison':
        return greet
    else:
        return 'no'

a = hi()
b = a()
```
此时a指向greet函数，但并没有执行这个函数。
b执行了greet函数，输出'in greet()'。


## 函数作为参数传给另一个函数
```py
def hi():
    return 'hi'

def before_hi(f):
    print('before')
    print(f())

before_hi(hi)
```
将hi函数作为参数传入另一个函数中，并在函数中调用。


## 装饰器
```py
def dec(f):
    def wrap_func():
        # before func
        f()
        # after func
    return wrap_func  # 这里返回里层的函数，但并不会调用。

def need_dec():
    print('need dec')

need_dec = dec(need_dec)
need_dec()
```

或者可以使用@符号来简化生成一个被装饰的函数。

```py
@dec
def need_dec():
    print('need dec')

deed_dec()
```
使用@注解只是简化了need_dec = dec(need_dec)这一步。

```py
def dec(f):
    @wrap(f)  # 保留原函数的所有属性
    def wrap_func():
        # before func
        f()
        # after func
    return wrap_func
```
经过装饰器后，由于返回的里层的wrap_func，所以拿到的函数变为了里层的wrap_func。使用`@wrap`可以保留原函数的所有属性。

如果想要带参数的装饰类，则需要再创建一个函数，然后在内嵌套装饰器。


## 装饰类
```py
class logit():
    def __init__(self, logfile='out.log'):
        self.logfile = logfile
    
    def __call__(self, func):
        @wrap(func)
        def wrap_func(*args, **kwargs):
            ...
            return func(*args, **kwargs)
        return wrap_func
    
    def notify(self):
        raise NotImplementError
```
优点：
- 如果想设计带参数的装饰类，不需要使用函数嵌套装饰器。
- 可以创建一个基类，装饰过程中可能会用到一些重写的方法，通过子类继承重写。


## 多个装饰器的执行顺序
先会执行离需要装饰的函数近的装饰器。但是，内部的wrap_func函数中执行的顺序其实是先执行离函数最远的装饰器中的内容。

相当于先依次调用装饰器内的东西，再从内到外层层封装。

```py
def dec_a(func):
    print('dec_a')
    def inner_a():
        print('inner_a')
        func()
    return inner_a

def dec_b(func):
    print('dec_b')
    def inner_b():
        print('inner_b')
        func()
    return inner_b

@dec_b
@dec_a
def need_dec():
    print('need_dec')

# dec_a
# dec_b
# inner_b
# inner_a
# need_dec
```

如果装饰器内**没**东西执行，可以认为**从上到下**依次执行装饰器。


## 总结
1. 装饰器可以在每个函数的执行前或执行后进行一些其他操作。
2. 被装饰的函数可以在内部函数内执行，也可以作为内部函数的返回值，但是**一定要调用这个函数**。
3. 装饰类与装饰函数其实都是使用了对象的思想，个人认为装饰类的实现更优，更简单也更灵活。
4. 装饰器的常见应用：授权，日志。





----
Reference: https://www.runoob.com/w3cnote/python-func-decorators.html