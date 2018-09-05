---
title: JS转换成数字
date: 2018-09-04 21:29:53
tags: ['JS']
categories: ['JS']
---

JS字符串转换成数字的方法有如下三种:

1. `parseInt()` 或者 `parseFloat()`
2. `Number(value)`
3. ` + - * / `

### 1. parseInt() 与 Number() 的区别

`parseInt()` 会将带数字与字符型的字符串也转换为数字，还是看几个例子吧

``` javascript
parseInt("123abc");  // 123
parseInt(100, 10);  // 100
parseInt(100, 2);  // 4， 额外的表示进制的参数

Number("100");  // 100
Number("100abc");  // NaN
```

### 

### 2. 运算符转换

``` javascript
1 + "1";  // 11
1 - "1";  // 0
1 * 1;    // 1
1 / 1;    // 1
```



