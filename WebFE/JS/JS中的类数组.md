---
title: JS中的类数组对象
date: 2018-08-23 22:36:53
tags: ['JS', 'JS数组']
categories: ['JS']
---

有几个点需要掌握：

1. 什么是类数组
2. 哪些常见的类数组
3. 如何判断一个对象是不是类数组
4. 如何将类数组转换为数组

## 1. 什么是类数组

1. 拥有`length`属性以及可以通过整数下标`0,1,2...`等访问成员
2. 但是其不用有数组所拥有的`indexOf,slice...`等方法

## 2. 哪些常见的类数组

1. 函数的参数列表 `arguments`

![1535084093714](D:\NoteBook\WebFE\JS\JS中的类数组.assets\1535084093714.png)

2. document.getElementsByClassName

   ![1535084324157](D:\NoteBook\WebFE\JS\JS中的类数组.assets\1535084324157.png)

我们可以看到获取到的结果是一个`HTMLCollection`

3. document.getElementsByTagName

![1535084441165](D:\NoteBook\WebFE\JS\JS中的类数组.assets\1535084441165.png)

同样获取到的是一个`HTMLCollection`

4. document.querySelectorAll

   ![1535084548299](D:\NoteBook\WebFE\JS\JS中的类数组.assets\1535084548299.png)

我们可以发现其获取到的是一个`NodeList`



## 3. 如何判断类数组

### 1. jQuery给出的判断方式

``` javascript
// 参考$.makeArray
function isArrayLike(obj){
    // 这里由两个与操作符并列的三个条件中前两个返回bool值用于判断obj本身是否为null、undefined
    // 或者obj是否含有length属性，都为true则返回obj.length
    let length = !!obj && length in obj && obj.length;  
    let type = jQuery.type(obj);
    
    if(type === "function" || jQuery.isWindow(obj)) return false;
    
    // 这里主要要判断的点是
    // 1. 如果是array，则直接返回true
    // 2. 若不是array，则判断length是否为0，为0也返回true
    // 3. 如果既不是array，length也不是0，那么判断其是不是整数，且obj[length - 1]是否存在
    return type === 'array' || length === 0 || 
        typeof(length) === "number" && length > 0 && obj[ length - 1 ]
}
```



### 2. underscore给出的判断方式



## 4. 如何将类数组转换为数组

### 1. 通过slice方法

我们通过`Array.prototype.slice.call`来转换

等同于 `[].slice.call`

转换前	

![1535099406159](D:\NoteBook\WebFE\JS\JS中的类数组.assets\1535099406159.png)

转换后

![1535099338918](D:\NoteBook\WebFE\JS\JS中的类数组.assets\1535099338918.png)



### 2. ES6中我们可以使用Array.from

我们直接给`Array.from`传入我们需要转换的类数组作为参数就可以了