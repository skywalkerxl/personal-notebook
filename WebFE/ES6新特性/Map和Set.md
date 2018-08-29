---
title: Map和Set数据结构
date: 2018-08-29 09:49:18
tags: [JS, ES6]
categories: [JS, ES6]
---

# Map和Set数据结构

本文主要解决以下几个问题：

1. `Map`和`Set`是什么
2. ES6提出`Map`和`Set`的目的
3. `Map`的操作方法
4. `Set`的操作方法
5. `Weak Map `与  `Weak Set`

## 一、Map和Set是什么

`Map`是ES6中键值对构成的有序列表，其中键和值可以是任意类型，如果想要比较键之间的不同，使用`Object.is()`

``` javascript
let m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
```

![1535511182597](https://wave-1253805456.cos.ap-chengdu.myqcloud.com/WaveBlog/1535511182597.png)

`Map`的浏览器兼容性

![1535511431742](https://wave-1253805456.cos.ap-chengdu.myqcloud.com/WaveBlog/1535511431742.png)

`Set`是一种**无重复值**的有序列表

``` javascript
let s = new Set([1,2,3,4,5,5]);
```

![1535511001401](https://wave-1253805456.cos.ap-chengdu.myqcloud.com/WaveBlog/1535511001401.png)

`Set`的浏览器兼容性

![1535511636832](https://wave-1253805456.cos.ap-chengdu.myqcloud.com/WaveBlog/1535511636832.png)

除了IE的兼容性比较差之外，移动及其它PC端浏览器都予以支持

## 二、ES6提出Map和Set的目的

提出`Map`和`Set`的目的其实就是用于弥补当前数据结构的不足

我们可以把对象与`Map`做对比，将数组与`Set`做对比

### 1. object 与 Map的区别

object的键只能为 `string, number, symbol`，而Map的键可以为任意类型，数组对象函数都可以

object可以通过对象字面量或者new构造函数来创建，而Map只能实例化构造函数来创建

Map有内置的迭代器，而object是没有的

### 2. 数组与Set的区别

其实只要理解了`Set`是一个无重复的有序集合就好了，那么他与数组的一个区别其实就是一个无重复，这里也就涉及到了利用Set来去重



那么在这些区别的背后，ES6提出了这两项新的数据结构的目的则是为开发者提供了更多的选择。

## 三、Map的操作方法

``` javascript
// 初始化Map
let m = new Map();
// 1.增加 map.set(key, value)
m.add(name, "lang");  // Map { name => "lang" }
// 2.删除 
// 可以使用 delete, clear
// 其中clear与delete的区别在于，clear会清空map，而delete是删除属性
map.delete(name);
map.clear();
// 3. 修改
map.set("name");

// 4. 查找
// has可以用来判断是否拥有该值
// get可以获取该值
map.has("name");
map.get("name");

// 5. 遍历
// 可以通过forEach来遍历
// @params value 值
// @params index 键
// @params owner map本身
map.forEach((value, index, owner)=> {
    
});
// 也可以通过for...of来遍历
```



## 四、Set的操作方法

``` javascript
// 初始化Set
let s = new Set();
// 1.增加 s.add()
s.add("lang");

// 2.删除
// 删除可以使用s.clear() 或者 s.delete()
s.remove("lang");  // 移除
s.clear();  // 移除所有项

// 3.修改
// Set的修改没啥好说的


// 4.查询
// has可以判断是否拥有该值
s.has("lang");

// 5.遍历
// 可以使用forEach来遍历，需要注意的是，他的第一个参数和第二参数相同，也就是说
// 遍历时的每一项既是值也是键
// 我们还可以通过for...of来遍历
```



## 五、Weak Map 和 Weak Set

`Weak Map`的一个主要目的是为了防止内存泄露，键所对应的对象在未来都有可能消失，比如说DOM对象。

`Weak Set`的一个主要目的也是为了方便垃圾回收