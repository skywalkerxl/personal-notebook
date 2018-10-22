---
date: 2018-10-22 11:47:30
title: Applition分析
tags: [NodeJS, koa2]
categories: [NodeJS]
---

找到 `node_modules/lib` 中的 `application.js` 文件

### 1. 依赖部分

我们从依赖开始分析

``` javascript
/**
 * Module dependencies.
 */
const isGeneratorFunction = require('is-generator-function'); // 判断是否是一个标准的 Generator 函数
const debug = require('debug')('koa:application');  // 一个常用的 debug 库
const onFinished = require('on-finished');  // 当 http 结束或出错时，调用该函数
const response = require('./response');  // http 的响应
const compose = require('koa-compose');  // 中间件的函数数组
const isJSON = require('koa-is-json');  // 判断是否是 json 数据
const context = require('./context');  // 运行的上下文
const request = require('./request');  // http 请求
const statuses = require('statuses');  // 请求状态
const Emitter = require('events');  // Node 中的 EventEmitter
const util = require('util');  // Node 的 util 类库
const Stream = require('stream');  // Node 中的 Stream 流
const http = require('http');  // Node 中的 http 库
const only = require('only');  // 对象的白名单属性选择
const convert = require('koa-convert');  // 兼容老版本 koa 的中间件 generator 做兼容，转成标准的中间件
const deprecate = require('depd')('koa');  // 标记不赞成或废弃的 api 等
```

### 2. Application类中的方法

该类本身继承于 Node 中的 `Emitter` 类

包括的方法如下：

``` javascript
class Application extends Emitter {
    constructor() { 
        super();
        this.proxy = false;
        this.middleware = [];
        this.subdomainOffset = 2;
        this.env = process.env.NODE_ENV || 'development';
        this.context = Object.create(context);
        this.request = Object.create(request);
        this.response = Object.create(response);
        if (util.inspect.custom) {
          this[util.inspect.custom] = this.inspect;
        }
    }  // 构造函数
    listen(..args) {
        const server = http.createServer(this.callback());
        return server.listen(...args);
    }  // 生成一个 http 服务，传入的内容包括端口号，ip地址等等
    toJSON() {  }
    inspect() {  }
    use(fn) { }  // 这里传入的 async 函数会被直接添加至中间件 middlewares 当中
    callback() {  }  // 这里主要是 this.handleRequest 对中间件和上下文的处理
    handleRequest(ctx, fnMiddleware) {  }  // 这一部分主要是对传入的请求做处理 
    createContext(req, res) {  }  // 创建一个上下文，主要针对 request, response, context 部分
    onerror(err) {  }  // 异常处理
}
```

