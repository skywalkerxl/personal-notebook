---
date: 2018-10-25 09:41:24
title: NodeJS 的 EventLoop
tags: [NodeJS]
categories: [NodeJS]
---

这部分简单描述下 NodeJS 的 EventLoop

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