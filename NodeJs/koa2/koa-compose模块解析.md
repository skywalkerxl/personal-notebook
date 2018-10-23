---
date: 2018年10月23日11:22:35
title: koa-compose 模块解析
tags: [NodeJS, koa2]
categories: [NodeJS]
---

`koa-compose` 用于加载 koa 的中间件

先上源码

``` javascript

module.exports = compose

/**
 * Compose `middleware` returning
 * a fully valid middleware comprised
 * of all those which are passed.
 *
 * @param {Array} middleware
 * @return {Function}
 * @api public
 */

function compose (middleware) {
  if (!Array.isArray(middleware)) throw new TypeError('Middleware stack must be an array!')
  for (const fn of middleware) {
    if (typeof fn !== 'function') throw new TypeError('Middleware must be composed of functions!')
  }

  /**
   * @param {Object} context
   * @return {Promise}
   * @api public
   */

  return function (context, next) {
    // last called middleware #
    let index = -1
    return dispatch(0)
    function dispatch (i) {
      if (i <= index) return Promise.reject(new Error('next() called multiple times'))
      index = i
      let fn = middleware[i]
      if (i === middleware.length) fn = next
      if (!fn) return Promise.resolve()
      try {
        return Promise.resolve(fn(context, dispatch.bind(null, i + 1)));
      } catch (err) {
        return Promise.reject(err)
      }
    }
  }
}

```

当然我们在看核心代码时需要去掉这些边边角角的代码

我们去掉代码中的注释和一些防御性代码(defensive code)，最后得到的结果如下，

``` javascript
function compose (middleware) {
  return function (context, next) {  // 这里返回了一个匿名函数，匿名函数返回的真正结果是一个 Promise
    let index = -1
    return dispatch(0)
    function dispatch (i) {
      index = i
      let fn = middleware[i]  // 遍历时用 fn 获取中间件数组中所对应中间件信息
      // 这里返回一个 resolve 状态的 Promise
      // 第二参数作为 next 传入中间件，同样也是个 dispatch 函数，也即一个 Promise 对象
      // 这样也就意味着，他在一个 Promise 函数中 await 下一个 Promise 函数
      // 此外再继续 await 下一个 Promise，直到最后
      return Promise.resolve( fn(context, dispatch.bind(null, i + 1)) );         
    }
  }
}
```

这里我们可以这样来走一遍流程

假设我们有三个如下的中间件

``` javascript
const mid1 = async ( ctx, next ) => {
    ...
    await next();
};
const mid2 = async ( ctx, next ) => {
    ...
    await next(); 
}
const mid3 = async ( ctx, next ) => {
    ...
    await next();
}
```

在 `app.use`中，我们将这三个中间件依次 `push` 进了 `this.middlewares`数组中

`compose`函数中传入 `middlewares`数组

`compose`函数中返回的是 `dispatch`函数

`dispatch`函数传入索引值，函数内定义变量 `fn` 接受当前执行的`middleware`，返回一个 `resolve`状态的 `Promise`

同时我们给该中间件函数`fn`传入两个参数

一个是当前上下文 `ctx`，另一个则是下一个 `dispatch`函数，

