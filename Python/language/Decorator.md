```python

def my_decorator(func):
    def wrapper():
        print('wrapper of decorator')
        func()
    return wrapper

def greet():
    print('hello world')

greet = my_decorator(greet)
greet()

# 输出
wrapper of decorator
hello world
```

||

```python

def my_decorator(func):
    def wrapper():
        print('wrapper of decorator')
        func()
    return wrapper

@my_decorator
def greet():
    print('hello world')

greet()
```



```python

def my_decorator(func):
    def wrapper(*args, **kwargs):
        print('wrapper of decorator')
        func(*args, **kwargs)
    return wrapper
```

*args和**kwargs，表示接受任意数量和类型的参数



// ankitodo

Q: 被装饰的函数, 保留原函数的元信息(也就是将原函数的元信息, 拷贝到对应的装饰器函数里)

A:使用内置的装饰器`@functools.wrap`

```python

import functools

def my_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('wrapper of decorator')
        func(*args, **kwargs)
    return wrapper
    
@my_decorator
def greet(message):
    print(message)

greet.__name__

# 输出
'greet'
```



类装饰器

```python

class Count:
    def __init__(self, func):
        self.func = func
        self.num_calls = 0

    def __call__(self, *args, **kwargs):
        self.num_calls += 1
        print('num of calls is: {}'.format(self.num_calls))
        return self.func(*args, **kwargs)

@Count
def example():
    print("hello world")

example()

# 输出
num of calls is: 1
hello world

example()

# 输出
num of calls is: 2
hello world
.
```

装饰器的嵌套

```python

@decorator1
@decorator2
@decorator3
def func():
    ...
    

decorator1(decorator2(decorator3(func)))
```

执行顺序从里到外.:question:

```python

import functools

def my_decorator1(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('execute decorator1')
        func(*args, **kwargs)
    return wrapper


def my_decorator2(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('execute decorator2')
        func(*args, **kwargs)
    return wrapper


@my_decorator1
@my_decorator2
def greet(message):
    print(message)


greet('hello world')

# 输出
# execute decorator1
# execute decorator2
# hello world
```

装饰器应用场景

1. 身份认证
2. 日志记录
3. 输入合理性检查



## Summary

装饰器本质上是一个python函数或类

它可以让其他函数或类在不修改原有代码的情况下增加额外的功能. 

装饰器的返回值也是一个函数/类对象

适用场景: 插入日志、性能测试、事务处理、缓存、权限校验

