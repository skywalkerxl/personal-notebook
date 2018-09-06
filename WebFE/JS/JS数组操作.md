---
title: JS数组操作
date: 2018-09-04 21:56:59
tags: [JS]
categories: [JS]
---

JS数组操作大致可以分为以下几类

### 1. 增加

数组中增加数据的方法有

* `push` 这种方法类似于栈的入栈，返回的是新数组的长度
* `unshift` 这种方法类似于队列的入队，返回新数组的长度
* `splice(startIndex, deleteCount, item)`  

``` javascript
let arr = [1, 2, 3, 4, 5];

arr.splice(1, 0, 9);   // [1, 9, 2, 3, 4, 5]  注意，这里不是在原数据后插入，而是在给的位置上插入
```



### 2. 删

* `pop` 这种方法类似于栈的出栈（删除最后一个位置上的元素），返回被出栈的元素
* `shift`  这种方法类似于队列的出队（删除第一个位置上的元素），返回被出队的元素
* `splice(deletePos, deleteCount)`  这种方法可以删除数组中指定位置的元素

### 3. 修改

* 这个直接取下标赋值就可以了
* `(ES6) fill(value, startIndex, endIndex)` 使用: `[1,2,3,4].fill(0, 1, 2);  // [1,0,0,1]`
* `(ES6) copyWithin(pasteIndex, copyIndex, stopPasteIndex)`

这里我们可以这样来使用，这里注意 `copyWithin` 中 `in` 为小写

``` javascript
let arr = [1, 2, 3, 4, 5];

// 第一个参数 2，表示要粘贴的位置
// 第二个参数 0 和第三个参数 2 组成了要复制的内容，即从 0 复制到 1 (到2停止)
arr.copyWithin(2, 0, 2);  // [1, 2, 1, 2, 5];
```



### 4. 查询

* 这个直接通过下标查询就可以了
* `indexOf(value)`   value 是你想要查询的数，返回是 value 所在数组中的位置
* `(ES6) find()` 使用方式如下:  `[25, 30, 35, 40].find(n => n > 33);  // 35 `
* `(ES6) findIndex()` 使用方式如下： `[25, 30 ,35, 40].findIndex(n => n > 33)  // 2`

### 5. 遍历

有一点需要注意的是，下面的这些迭代方法都不会影响原数组

* `forEach(elt, index, arr)`  
* `filter` 返回一个新的满足条件的数组
* `some(elt, index, arr)`  是否存在元素通过了测试用例
* `every(elt, index, arr)` 是否所有元素通过了测试用例
* `map(elt, index, arr)`  
* `reduce(pValue, cValue, cIndex, cArr)` 

这里需要明确下 `forEach` 和 `map` 的区别

`forEach` 主要是用于遍历

`map` 更通常的用法和翻译为**映射**， 比如说 react 我们通过一个数组来生成一组列表并返回，那么这就是一种映射

### 数组排序

数组的排序会修改原数组，返回的是数组的地址

* `reverse` 反转数组
* `sort` 数组排序

### 6. 数组扁平化

比如说我们有这样一个需求，是将 `[[1, 2], [3, 4],[5, 6],[7, 8]]` 展开为 `[1, 2, 3, 4, 5, 6, 7, 8]`

那我们可以这样做

``` javascript
let arr = [[1, 2], [3, 4],[5, 6],[7, 8]];

arr.reduce( (pValue, cValue, cIndex) => {
    return pValue.concat(cValue);
});
```



### 7. 数组字符串化

* `join`

``` javascript
let arr = [1, 2, 3];
arr.join("|");  // 1|2|3
```

* `toString`

``` javascript
let arr = [1, 2, 3];
arr.toString();  // 1,2,3
```



### ES6 新增数组操作

* `Array.of()`  这是在创建数组的时候使用
* `Array.from()`  这是用于将类数组转换为数组，还可以在回调函数中增强原数组返回新数组
* `[1, 2, 3].find()`
* `[1, 2, 3].findIndex()`
* `[1, 2, 3].fill(value, startIndex, endIndex)`  用 value 来填充数组 
* `copyWithin`