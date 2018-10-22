---
date: 2018-10-22 16:19:23
title: koa2API分析-Context
tags: [NodeJS, koa2]
categories: [NodeJS]
---

这一部分是对 `koa2` 中 `Context` 部分的分析

### 一、依赖部分

``` javascript
/**
 * Module dependencies.
 */

const util = require('util');  // Node 提供的工具库
const createError = require('http-errors');  // 该部分是为了方便的创建 http 错误信息
const httpAssert = require('http-assert');  // 状态码断言
const delegate = require('delegates');  // 一个方便委托方法和访问其的库
const statuses = require('statuses');  // HTTP 状态码库
const Cookies = require('cookies');  // cookies操作

const COOKIES = Symbol('context#cookies');

```

### 二、提供的方法

`Context` 本身返回的其实很简答，就是一个 `const proto = module.exports = { ... }`

模块中大体代码如下：

``` javascript
const proto = module.exports = {
    inspect(){ },
    toJSON(){ },
    assert: httpAssert,
    throw(...args){ },
    onerror(err){ },
    get cookies() { }
    set cookies() { }
}

// 这部分是将 response 上的属性和方法等委托到 proto 上
delegate(proto, 'response')
	.method('...')

// 这部分是将 request 上的属性和方法等委托到 proto 上
delegate(proto, 'request')
	.method('...')

```

