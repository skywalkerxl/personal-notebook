---
title: 装饰器 decorator
date: 2018-09-15 19:58:07
tags: [python, 函数式编程]
categories: [装饰器]
---

python 里面有个装饰器的功能，为装饰模式的语法糖

先写一个不带参数的装饰器，计算函数的时间

``` python
def time_spend(func):
    def wrapper(*args, **kwargs):
        time_start = time.time()
        back = func(*args, **kwargs)
        print (time.time() - time_start)
        return back;
    return wrapper()
```





