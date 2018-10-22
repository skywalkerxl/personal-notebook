---
title: JS正则表达式
date: 2018-09-05 10:12:05
tags: [JS, 正则表达式]
categories: [JS, 正则表达式]
---

JS 正则表达式

## 基本使用

该部分参考 [《Javascript 正则表达式迷你书》](http://pbjzbmlq3.bkt.clouddn.com/JavaScript%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%BF%B7%E4%BD%A0%E4%B9%A6.pdf)

首先我们要记住的一句话是 

`"正则表达式要么匹配字符，要么匹配位置"`

### 1. 字符的匹配

首先心里要有两个概念： 

* 横向模糊匹配
* 纵向模糊匹配

那怎么去理解这两个概念呢？

比如说我们有个需求是匹配字符 b ，如果有1到3个b字符就予以匹配，那这种做法就是**横向模糊匹配**

那又比如说我们有个需求是匹配字符a或者b或者c，那这种做法就是**纵向模糊匹配**

#### 横向模糊匹配

我们在横向模糊匹配中使用 `{m, n}` 的方式来匹配 字符出现 m 到 n 次的情况

``` javascript
let str = "abc abbc abbbc abbbbc abbbbbc abbbbbbc";
let reg = /ab{2, 5}c/g;
str.match(reg);  // [abbc, abbbc, abbbbc, abbbbbc]
```



#### 纵向模糊匹配

我们在纵向模糊匹配中使用 `[abc]` 的形式来匹配我们的字符

``` javascript
let str = "abzaczagz";
let reg = /a[bc]z/g;
str.match(reg);  // [abz, acz]
```



有了这两个概念之后，我们接下来思考一个问题

我们在上面的写法当中，把要匹配的部分都写的很固定，那如何让我们把要匹配的项变得灵活化呢？

这也就有了 **字符组**， **量词**， **多选分支** 的概念

#### 字符组

比如说我们目前想要匹配的字符不是 a b c 中的任意一个字符，而是为 任意一个小写字母，那我们可以这样来匹配，比如说



#### 量词





#### 多选分支





## 常见案例

### 1. 使用正则解析一个 URL

1. 指定参数名称，返回该参数的值 或者 空字符串
2. 不指定参数名称，返回全部的参数对象 或者 {}
3. 如果存在多个同名参数，则返回数组

``` javascript
function getUrlParam(url, key){
    let res = {};
    url.replace(/\??(w+)=(w+)\&?/g, (match, key, value) => {
        
    })
}
```



### 2. var s1 = "get-element-by-id"; 给定这样一个连字符串，写一个function转换为驼峰命名法形式的字符串 getElementById



### 3. 判断字符串是否包含数字



### 4. 判断电话号码



### 5. 判断是否符合指定格式

给定字符串str，检查其是否符合如下格式

1. XXX-XXX-XXXX
2. 其中X为Number类型

### 6. JS实现千位分隔符

``` javascript
function format(number) {     
    // 这里匹配的思想
    // 找到元素开头是 1 - 3 位的数字，这里是为了不在字符串的开头填充逗号
    // 以元素中 3 位数字为一组，元素前填充逗号
    var regx = /\d{1,3}(?=(\d{3})+$)/g;  
    // 或者我们这样做，不匹配元素开头的位置，可以这样写
    var regx2 = /(?!^)((\d{1,3})+)$/g;
    return (number + '').replace(regx, '$&,')  // $&表示与regx相匹配的字符串 
}
```

### 7. 验证邮箱



### 8. 验证身份证号码



### 9. 匹配汉字



