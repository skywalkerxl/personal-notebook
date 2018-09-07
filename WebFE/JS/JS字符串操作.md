---
title: JS字符串操作
date: 2018-09-05 11:21:15
tags: [JS]
categories: [JS]
---

JS字符串操作

我们将字符串操作分为以下几类

### 增加



### 删除



### 修改

* `toLowerCase()`  字符串英文全部转小写，不影响本身
* `toLocaleLowerCase()`  大多数情况下与 `toLowerCase()` 表现一致
* `toUpperCase()`  字符串英文全部转大写，不影响本身
* `toLocaleUpperCase()`  大多数情况下与 `toUpperCase()` 表现一致
* `replace()`  偏向于正则的用法，放在正则部分
* `split(char)`   通过参数分割字符串，不影响原数组
``` javascript
let str = "a,b,c,d,e,f";
let str1 = "a b c d e f";
str.split(',');   // [a, b, c, d, e, f]
str1.split(' ');  // [a, b, c, d, e, f]
```

* `trim()` 去除空格
* `(ES6) padStart(num, str)` 头部补齐
``` javascript
'wa'.padStart(6, "he");  // "hehewa"
'wa'.padStart(5, "he");  // "hehwa"
'wa'.padStart(4, "he");  // "hewa"
'wa'.padStart(3, "he");  // "hwa"
'wa'.padStart(2, "he");  // "wa"
'wa'.padStart(1, "he");  // "wa"
```
* `(ES6) padEnd(num)` 尾部补齐
``` javascript
'wa'.padEnd(4, "he");  // "wahe"
'wa'.padEnd(3, "he");  // "wah
'wa'.padEnd(2, "he");  // "wa"
'wa'.padEnd(1, "he");  // "wa"
```


### 查找

* `charAt(num)` 返回的是该位置的字符

``` javascript
let str = "hello wave";
str.charAt(1);  // 获取的是 e
```

* `charCodeAt(num)`  返回的是该位置的字符编码

``` javascript
let str = "hello wave";
str.charCodeAt(1);  // 获取的是 e 的 ascii 码 101
```

* `slice(startIndex, endIndex)`  从源字符串中提取字符串并返回新字符串，不影响原字符串

``` javascript
let str = "hello wave";
str.slice(1);      // ello wave
str.slice(1, 2);   // e  这里不包括第二个字符
str.slice(1, -1);  // ello wav
str.slice(3, 1);   // "" 空字符串
```

* `substr(startIndex, length)`   从源码中截取字符串

``` javascript
let str = "hello wave";
str.substr(1, 2);    // el
str.substr(-3, 2);   // av
str.substr(1);       // ello wave
str.substr(-20, 2);  // he
str.substr(20 ,2);   // ""
```

* `substring(startIndex, endIndex)`   会将负值置为0，且如果第二个参数大于第一个参数，会交换两个参数
* `indexOf(value)`   返回指定值第一次出现的位置，没有就返回 -1
* `lastIndexOf(value)`   返回指定值最后一次出现的位置，没有就返回  -1
* `(ES6) includes(str, position)`  返回是否搜索到了 str ，有就返回`true`，没有则返回`false`pisition 可选表示位置
* `(ES6) startsWith(str, position)`  判断当前字符串是否由 str 开头， position 可选，表示位置

``` javascript
'w'
```



* `(ES6) endsWith()`
* `(ES6) matchAll()`

### 遍历

* `(ES6) for...of`



### 合并

* `concat` 合并多个数组



