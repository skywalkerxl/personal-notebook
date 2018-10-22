---
title: JS中抽象相等和 toPrimitive
date: 2018-09-18 10:21:20
tags: [JS]
categories: [JS]
---

我们首先要理清JS中的基本的值类型

`Undefined`、`String`、`Boolean`、`Number`、`Null`、`Symbol`

判断规则如下：

1. 如果 x 与 y 的类型相同，那么就相当于 x === y
2. 如果 x 为 undefined ，y 为 null，那么返回 true
3. 如果 y 为 null， x 为 undefined，那么返回 true
4. 如果 x 为 string, y 为 undefined， 那么返回 ToNumber(x) == y
5. 如果 y 为 undefined, x 为 number， 那么返回 x == ToNumber(y)
6. 如果 x 为 Boolean，那么返回 ToNumber(x) == y
7. 如果 y 为 Boolean，那么返回 x == ToNumber(y)
8. 如果 x 为 string, number, symbol ，y 为 object， 返回  x == ToPrimitive(y)
9. 如果 y 为 string, number, symbol ，x 为 object， 返回  ToPrimitive(x) == y

这里需要理解 ToPrimitive 的计算过程,

我们需要根据 valueOf 和 toString 来计算，如果我们 valueOf 返回的结果不是基本的值类型， 那么我们就执行 toString 操作来执行。



### 举例

一个比较经典的例子， [] == ![] 为什么返回是true ，而不是 false

先判断 ![] ，![] 返回的是个 Boolean 型，而且结果是 false

Boolean 会被转换为 Number, 也就是 0

再判断 [] == 0

而这里 [] 需要做 ToPrimtive 判断，valueOf的返回结果是 [] ，这显然不是个基本的值类型，那么执行 toString ,返回的结果是 `""` ， string 类型会被转换为 number，也即是 0

那么 显然 0 ==  0 是 true， 所以返回 true