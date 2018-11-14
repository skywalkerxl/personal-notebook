---
date: 2018-11-13 20:36:22
title: 职责链模式
tags: ['design pattern']
categories: ['design pattern']
---

职责链模式将请求的发送者和接收者解耦，且使得多个对象都可以处理该请求

定义`Chain`类来实现

``` javascript
function Chain(fn){
    this.fn = fn;
    this.successor = null;
}

Chain.prototype = {
    constructor: Chain,
    setNextSuccessor: function(successor){
        return this.successor = successor;  // 赋值并返回 successor
    },
    passRequest: function(){
        let ret = this.fn.apply(this, arguments);
        // 如果执行的结果为 nextSuccessor，那么请求后延
        if( ret === 'nextSuccessor' ){
            // 如果其下一个请求节点存在则返回其下一个节点的 passRequest 
            // 如果其下一个节点不存在，则返回 null
            return this.successor && this.successor.passRequest.apply(this.successor, arguments)
        }
        // 否则直接返回结果
        return ret;
    }
}
```

对 `Chain` 类的使用

``` javascript
function nodeFn(){
    // 对于每一个请求节点，都需要返回一个特定的 nextSuccessor 来确保将请求后移
    ...
    return 'nextSuccessor';
}
```

我们不妨假设有如下三个接收请求的节点 `nodeFn1`,`nodeFn2`,`nodeFn3`

``` javascript
function nodeFn1() { return 'nextSuccessor'; }
function nodeFn2() { return 'nextSuccessor'; }
function nodeFn3() { return 'nextSuccessor'; }

let chain1 = new Chain(nodeFn1);  // 定义第一个节点
let chain2 = new Chain(nodeFn2);  // 定义第二个节点
let chain3 = new Chain(nodeFn3);  // 定义第三个节点

chain1.setNextSuccessor(chain2);
chain2.setNextSuccessor(chain3);

// 将请求传给第一个节点
chain1.passRequest(params1); 
chain1.passRequest(params2); 
chain1.passRequest(params3);
```

我们来梳理下这段代码执行的流程。

首先 `chain1.passRequest(params1)`会执行 `chain1.fn` 也即 `nodeFn1`，根据返回的结果是否有`nextSuccessor`来判断是否进行下一步，这里我们默认其返回了 `nextSuccessor`

之后便会根据其是否有 `successor`来判断是否继续执行其`successor`的`passRequest`

也即 `chain1.passRequest ---> chain2.passRequest`

## 异步职责链

在上一部分，我们的职责链节点间的调用是自动同步执行调用的，在一些情况下会有异步调用的情况，那就需要我们手动触发对下一个节点的调用，改写`Chain`类代码如下

``` javascript
Chain.prototype = {
    constructor: Chain,
    setNextSuccessor: function(successor){
        return this.successor = successor;
    },
    next: function(){
        return this.successor && this.passRequest.apply(this.successor, arguments);
    }
}
```

同时节点函数中也不再需要返回`"nextSuccessor"`



## AOP实现职责链

这里我们重写`Function.prototype.after`如下

``` javascript
Function.prototype.after = function(fn){
    let self = this;
    return function(){
        let ret = self.apply(this, arguments);
        if(ret === 'nextSuccessor'){
            return fn.apply(this, arguments);
        }
    	return ret;
    }
}
```

使用方式如下

``` javascript
let node = nodeFn1.after(nodeFn2).after(nodeFn3);

node(params1);
node(params1);
node(params1);
```

