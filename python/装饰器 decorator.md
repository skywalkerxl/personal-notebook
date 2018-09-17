---
title: 装饰器 decorator
date: 2018-09-15 19:58:07
tags: [python, 函数式编程]
categories: [装饰器]
---

python 里面有个装饰器的功能，为装饰模式的语法糖

先写一个不带参数的装饰器，计算函数的时间

``` python
# 这里是计算时间的装饰器
def time_spend(func):
    def wrapper(*args, **kwargs):
        time_start = time.time()
        back = func(*args, **kwargs)
        print (time.time() - time_start)
        return back;
    return wrapper()
```

我们可以通过这样的方式来调用

``` python
@time_spend():
    def demo_func():
        time.sleep(1)
        print("我结束了")
```









