---
date: 2018-10-16 09:23:01
title: Strategy Pattern
tags: [design Pattern]
categories: [design Pattern]
---

策略模式的核心是**定义一系列的算法，把它们一个个封装起来，并且使他们可以相互转换**

## 一、用策略模式重构表单校验

在重构之前，我们来看下最后要达到的效果是怎样的

``` javascript
// 使用方式
let validator = new Validator();

// 添加验证规则如下 
validator.add(registerForm.userName, 'isNonEmpty', '用户名不可为空');
validator.add(registerForm.password, 'minLength:6', '最小长度不可少于6');
validator.add(registerForm.phoneNumber, 'isMobile', '手机号码格式不正确');

// 开始验证，错误值返回给errorMsg
let errorMsg = validator.start();
```



首先定义我们的策略

``` javascript
let strategies = {
    // 是否为空
    isNonEmpty: function( value, errorMsg){  
        if(value === ''){
            return errorMsg;
        }
    },
    // 最小长度
    minLength: function( value, length, errorMsg){
        if(value.length < length){
            return errorMsg;
        }
    },
    // 是否为手机号码
    isMobile: function( value, errorMsg){
        if(!/^(1[3|5|8][0-9]{9})$/.test(value)){
            return errorMsg;
        }
    }
}
```

其次定义验证类

``` javascript
function Validator (){
    this.cache = [];
}

Validator.prototype = {
    constructor: Validator,
    // 添加验证规则
    add: function(dom, rule, errorMsg){
        let ary = rule.split(':');  // 将策略与参数分开
        this.cache.push(function(){
            let strategy = ary.shift();  // 出队拿到具体的策略
            ary.unshift(dom.value);  // 将表单传来的值入队
            ary.push(errorMsg);  // 将发生错误时提示的信息入栈
            return strategies[ strategy ].apply( dom, ary);  // 返回该策略的执行结果并push到cache中
        })
    },
    // 开始验证
    start: function(){
        for(let msg of this.cache){
            if(item){
                return msg;
            }
        }
    }
};
```

