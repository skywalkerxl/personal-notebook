---
date: 2018-10-22 18:15:27
title: koa2API分析-Request
tags: [NodeJS, koa2]
categories: [NodeJS]
---

`Request` 模块本身是对请求的封装，首先还是依赖部分

``` javascript
/**
 * Module dependencies.
 */

const URL = require('url').URL;  // Node 的提供的 url 处理和解析的模块
const net = require('net');  // Node 提供的网络部分 API
const accepts = require('accepts');  // 请求头 accepts 的处理
const contentType = require('content-type');  // 创建与解析 HTTP Content-type 部分
const stringify = require('url').format;  // url 模块提供的可自定义序列化的 URL 字符串表达式
const parse = require('parseurl');  // 解析 url	
const qs = require('querystring');  // 解析和格式化查询字符串
const typeis = require('type-is');  // 请求的 content-type 类型
const fresh = require('fresh');  // 检测 http 请求新鲜度的库， 用于对缓存的判断
const only = require('only');
const util = require('util');

const IP = Symbol('context#ip');
```

### 方法及属性

该模块返回的也是个对象

``` javascript
module.exports = {
    get header() { }  // 返回请求头
    set header(val) { },  // 设置请求头
    get headers() { },  // 返回请求头
    set headers(val) { },  // 设置请求头
    get url() { },  // 获取请求的 url
    set url(val) { },  // 设置请求的 url
    get origin() { },  // 获取协议 protocol 及主机地址 host
    get href() { },  // 获取地址
    get method() { },  // 获取请求方式
    get pathname() { },  // 获取解析后的请求路径	
    ...  // 后面都是一些常用方法的封装
}
```

