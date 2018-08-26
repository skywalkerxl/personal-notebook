---
title: let const 和 var 的区别
date: 2018-08-07 13:30:15
tags: ['JS']
categories: ['JS']
---

基本区别：

1. let和const会产生块级作用域
2. let和const不存在变量的重复声明
3. let和const没有变量声明提升（也就是存在暂时性死区）

### 注意点：

const会阻止对于变量绑定和变量自身值的修改，也就是说这样使用会报错

1. 不可重复声明

``` javascript
const a = 1;
const a = 2;  // Uncaught SyntaxError: Identifier 'a' has already been declared
```

2. 不可修改变量的值

```  javascript
const b = 1;  
b = 2; // Uncaught TypeError: Assignment to constant variable. 
```

但是这样修改成员变量的值是不会报错的

``` javascript
const c = { a: 1 };
c.a = 2;
```