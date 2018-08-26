---
title: async && await
date: 2018-08-15 12:25:51
tags:
categories:
---



#异步最终解决方案async && await

几个比较重要的点：

1. async声明的是一个异步函数,返回的是一个promise
2. await只能用在async函数中
3. await如果执行的是一个同步语句，那会直接执行，否则会等待异步执行完成后再继续执行

