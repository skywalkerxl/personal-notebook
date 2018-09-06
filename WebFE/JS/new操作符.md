---
title: new 运算符的作用
date: 2018-08-01 08:44:36
tags: [JS]
categories: [JS]
---



new运算符 的作用

1. 首先创建一个新的空对象 `var obj = {}`
2. 其次会将构造函数的原型赋值到新对象的原型上，也即`obj.__proto__ = F.prototype`
3. 绑定this到新创建的对象上，也即`F.call(obj)`

我们可以通过这样的方式来实现一个new运算符

``` javascript
function Person(name){
    this.name = name;
}

function objectFactory() {

    // 这里新创建一个对象
    // 并获取传进来的 构造函数
    let obj = {},
        Constructor = [].shift.call(arguments);

    // 最关键的一步， 将新创建对象的 __proto__ 链到 构造函数的 prototype 上
    obj.__proto__ = Constructor.prototype;

    // 这里的作用是判断 构造函数本身会不会返回对象
    // 如果没有返回对象就用 上述步骤中新创建的对象
    // 否则就用返回的对象，所以这里判断了 tpyeof ret 是否为 object
    let ret = Constructor.apply(obj, arguments);
    return typeof ret === 'object' ? ret : obj;

}

let person = objectFactory(Person, "lang");

```

