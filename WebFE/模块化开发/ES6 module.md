---
title: ES6 Module
date: 2018-09-01 15:44:49
tags: [js, ES6]
categories: [js, ES6]
---

# ES6 Module

这篇文章可以帮助你解决如下几个疑问：

1. ES6 Module是运行时加载还是编译时加载（静态加载）？
2. 在不用babel等工具的情况下，我该如何直接使用ES6的Module？





### 1. ES6 Module是运行时加载还是编译时加载（静态加载）

ES6 Module为编译时加载，而nodejs的模块化规范CommonJs则是运行时加载，那这两种加载方式有什么区别呢？优缺点又是什么呢？

先说区别吧

`ES6 Module`在编译时就会加载，当一个模块引入另一个模块时，比如

``` javascript
import {a, b, c} from 'util.js';
```

那么实际引入的只有a,b,c，其它的则不会引入



此外ES6中的模块不是对象，而是显式输出的代码

那么优点则是在编译时可以做类型检查等



### 2. 在不用babel等工具的情况下，我该如何直接使用ES6的Module

node.js v9.x已经可以直接使用import export语法了，但是要给文件类型声明为`*.mjs`

