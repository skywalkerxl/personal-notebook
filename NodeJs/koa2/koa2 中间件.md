---
date: 2018-10-22 21:34:41
title: koa2 中间件
tags: [NodeJS, koa2, middlewares]
categories: [NodeJS]
---

koa2可以理解为一切均是中间件

我们尝试写一个这样的处理程序

``` javascript
const koa = require('koa');
const app = new koa();

// 中间件1
const mid1 = async (ctx, next) => {
    ctx.body = 'Hi, ';
    await next();
}

// 中间件2
const mid2 = async (ctx, next) => {
    ctx.type = "text/html; charset=utf-8";
    await next();
}

// 中间件3
const mid3 = async (ctx, next) => {
    ctx.body = ctx.body + "wave9";
    await next();
}

app.use(mid1);
app.use(mid2);
app.use(mid3);

app.listen(10090, () => {
    console.log(" app is running at port 10090 ");
});
```

这里我们通过 `app.use` 将中间件加载到 `middlewares` 数组当中

