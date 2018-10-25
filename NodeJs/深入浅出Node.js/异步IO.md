---
date: 2018-10-23 14:53:39
title: Node的异步I/O
tags: [NodeJS]
categories: [NodeJS]
---

`Node` 的 `异步 I/O` 是 `Node` 的一个重要特点

这部分重点理解 **Node的异步I/O** 和 **非I/O的异步API**

## 一、Node的异步I/O

Node的异步I/O模型，我们将他分为四个部分

1. 事件循环
2. 观察者
3. 请求对象
4. I/O线程池

发呢别说下这几个概念

### 1. 事件循环

Node会将回调事件放入一个循环当中，这个循环会一直执行下去，没执行一次循环体，我们就成为一次 `Tick`，`Tick` 的过程就是查看是否有事件待处理

### 2. 观察者

既然有了循环，循环里就会有待处理的事件，那是如何判断是否有事件待处理呢？

这就有了**观察者**的概念，观察者会来查看是否有待处理的事件，比如说网络I/O观察者、文件I/O观察者等等

### 3. 请求对象

请求对象其实是一种中间产物，这个中间产物有着很重要的作用，一方面是保存各种各样的状态，不管是 JavaScript 层面上传入的状态和回调函数会保存在里面，此外该产物还会被送到线程池中等待执行。

### 4. I/O线程池

有了I/O线程池，不管你是否阻塞 I/O，都不会阻塞原有的 JavaScript 的继续执行，这其实也就达到了异步的目的



## 二、非I/O的异步API

包括如下四个 API

1. `setTiemout()`
2. `setInterval()`
3. `setImmediate()`
4. `processs.nextTick()`

`setTimeout()` 和 `setTimeInterval()` 与浏览器环境中的API是一致的

### Process.nextTick()

这个API的作用很简单，就是将回调函数放入异步队列当中，再到下一轮的 `Tick` 中执行，

可以将其理解为 `setTimeout(fn, 0)`

代码如下

``` javascript
process.nextTick = function(){
    if(process._exiting) return;
    
    if(tickDepth >= process.maxTickDepth)
        maxTickWarn();
    
    var tock = { callback: callback };
    if(process.domain)
        tock.domain = process.domain;
    
    nextTickQueue.push(tock);
    
    if(nextTickQueue.length){
        process._nextTickCallback();
    }
}
```

### setImmediate()

这个API和 `Process.nextTick()`的作用极为相似，也是将事件放入事件循环当中

只不过 `Process.nextTick()` 的优先级要高于 `setImmediate()`，原因是`idle`的观察者优先于`check` 观察者

在具体表现上， `process.nextTick()`的回调函数是保存在数组当中，而 `setImmediate()` 是保存在链表当中，每一次 `Tick` 只会执行一个链表中的节点，这样可以保证每轮循环都能较快的结束，防止阻塞

## 三、六种 eventloop

``` javascript
const { readFile } = require('fs');
const EventEmitter = require('events');

class EE extends EventEmitter {}
const yy = new EE();

yy.on('event', () => {
    console.log('something is happening!');
});

setTimeout(() => {
    console.log("setTimeout 0ms callback");
}, 0);

setTimeout(() => {
    console.log("setTimeout 100ms callback");
}, 100);

setTimeout(() => {
    console.log("setTimeout 200ms callback");
}, 200);

readFile('./a.json', 'utf-8', () => {
    console.log("readFile 1st callback");
});

readFile('./a.json', 'utf-8', () => {
    console.log("readFile 2rd callback");
});

setImmediate(() => {
    console.log("setImmediate 1st callback");
});

setImmediate(() => {
    console.log("setImmediate 2rd callback");
});

process.nextTick(() => {
    console.log('process.nextTick 1st callback');
});

Promise.resolve()
    .then( () => {
    yy.emit('event');
    process.nextTick( () => {
        console.log('process.nextTick 2rd callback');
    });
    console.log('Promise 1st callback');
}).then( () => {
    console.log('Promise 2st callback');
});

```

我们从源码的角度来分析异步事件的执行顺序

[github地址](https://github.com/libuv/libuv/blob/v1.x/src/unix/core.c)

``` C++
int uv_run(uv_loop_t* loop, uv_run_mode mode) {
  int timeout;
  int r;
  int ran_pending;

  r = uv__loop_alive(loop);
  if (!r)
    uv__update_time(loop);

  while (r != 0 && loop->stop_flag == 0) {
    uv__update_time(loop);
    uv__run_timers(loop);  
    ran_pending = uv__run_pending(loop);
    uv__run_idle(loop);  
    uv__run_prepare(loop); 

    timeout = 0;
    if ((mode == UV_RUN_ONCE && !ran_pending) || mode == UV_RUN_DEFAULT)
      timeout = uv_backend_timeout(loop);

    uv__io_poll(loop, timeout); #
    uv__run_check(loop); 
    uv__run_closing_handles(loop); 

    if (mode == UV_RUN_ONCE) {
      /* UV_RUN_ONCE implies forward progress: at least one callback must have
       * been invoked when it returns. uv__io_poll() can return without doing
       * I/O (meaning: no callbacks) when its timeout expires - which means we
       * have pending timers that satisfy the forward progress constraint.
       *
       * UV_RUN_NOWAIT makes no guarantees about progress so it's omitted from
       * the check.
       */
      uv__update_time(loop);
      uv__run_timers(loop);
    }

    r = uv__loop_alive(loop);
    if (mode == UV_RUN_ONCE || mode == UV_RUN_NOWAIT)
      break;
  }

  /* The if statement lets gcc compile it to a conditional store. Avoids
   * dirtying a cache line.
   */
  if (loop->stop_flag != 0)
    loop->stop_flag = 0;

  return r;
}
```

从下面Node官网上的这张图来说，已经包含了上述代码中的示意

```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```

我们需要分清不同的异步API属于哪一部分的异步

1. **timers** ：包括 `setTimeout`,`setImmediate`
2. **pending callbacks**：
3. **idle, prepare**：
4. **poll**：
5. **check**：`setImmediate`
6. **close callbacks**：