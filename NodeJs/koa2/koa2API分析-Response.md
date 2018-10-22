---
date: 2018-10-22 18:58:38
title: koa2API分析-Response
tags: [NodeJS, koa2]
categories: [NodeJS]
---

`Response` 模块是对响应的封装

### 一、依赖部分

``` javascript
/**
 * Module dependencies.
 */

const contentDisposition = require('content-disposition');  // 对 Content-Disposition 的处理
const ensureErrorHandler = require('error-inject');  // 添加一个流的错误监听
const getType = require('cache-content-type');  // 对 cache-content-type 的处理
const onFinish = require('on-finished');  // 当 HTTP 请求关闭、结束或者出错时执行的回调函数 
const isJSON = require('koa-is-json');  // 判断是否是 JSON
const escape = require('escape-html');  // 对字符串中特殊字符的转义
const typeis = require('type-is').is;  // 请求的 content-type
const statuses = require('statuses');  // HTTP 状态码
const destroy = require('destroy');  // 这个模块是用于确保一个 stream 被彻底销毁了
const assert = require('assert');  // Node 提供的断言
const extname = require('path').extname;  // 返回 path 的扩展名
const vary = require('vary');  // 对 HTTP 头部 Vary 字段的分析，一种内容协商
const only = require('only');
const util = require('util');
```

### 二、对象内部的方法

`Response` 模块内部也是对响应的封装，大部分与 `Request` 模块处理的情形差不多

```javascript
module.exports = {
    set etag() { ... },  // 与 Last-Modified 相对应，主要做缓存方面的控制
    get etag() { ... },
    get type() { ... },  // 获取当前的文档类型
    is(types) { ... },  // 判断是否是某种类型
    get(fieldName) { ... },  // 获取配置项具体的内容
    set(fieldName, val) { ... },  // 设置 header 里面的子项内容
    append(field, val), { ... },  // 插入一些自定义的 header 配置项
    remove(field) { ... },  // 移除一些配置项
    get writable() { ... },  // 判断是否可写，
    flushHeaders() { ... },  // 刷新之前已经设置过的 headers
}
```

