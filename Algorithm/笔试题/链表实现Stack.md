---
title: js 链表实现 Stack
date: 2018-09-11 21:35:14
tags: [笔试题]
categories: [笔试题]
---

链表实现 Stack

```javascript
// 找的链式表示
function Stack() {
    this.top = null;
    this.size = 0;
}
module.exports = Stack;

Stack.prototype = {
    constructor: Stack,
    push: function (data) {
        let node = {
            data: data,
            next: null
        };

        node.next = this.top;
        this.top = node;
        this.size++;
    },
    peek: function () {
        return this.top === null ?
            null :
            this.top.data;
    },
    pop: function () {
        if (this.top === null) return null;

        let out = this.top;
        this.top = this.top.next;

        if (this.size > 0) this.size--;

        return out.data;
    },
    clear: function () {
        this.top = null;
        this.size = 0;
    },
    displayAll: function () {
        if (this.top === null) return null;

        let arr = [];
        let current = this.top;

        for (let i = 0, len = this.size; i < len; i++) {
            arr[i] = current.data;
            current = current.next;
        }

        return arr;
    }
};

let stack = new Stack();

stack.push(1);
stack.push('asd');

stack.pop();
stack.push({a: 1});
console.log(stack);
```