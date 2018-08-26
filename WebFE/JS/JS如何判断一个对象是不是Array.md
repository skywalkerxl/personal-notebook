# JS如何判断一个对象是不是Array

[Segment Fault原文链接](https://segmentfault.com/a/1190000006150186)

### 1. 用typeof运算符来判断

**typeof是js原生提供的判断数据类型的运算符，它会返回一个表示参数的数据类型的字符串**

typeof运算符的针对不同参数的输出结果的表格

| Type             | Result                   |
| ---------------- | ------------------------ |
| Undefined        | "undefined"              |
| Null             | "object"                 |
| Boolean          | "boolean"                |
| Number           | "number"                 |
| String           | "string"                 |
| Symbol(ES6新增)  | "symbol"                 |
| Host Object      | Implementation-dependent |
| Function Object  | "function"               |
| Any other object | "object"                 |

数组被归为Any other object,显示的结果为object

### 2. 用instanceof判断

**instanceof运算符可以用来判断某个构造函数的prototype属性所指向的对象是否存在于另外一个要检测对象的原型链上**

```javascript
const a = [];
const b = {};
console.log(a instanceof Array);   // true
console.log(a instanceof Object);  // true
console.log(b instanceof Array);   // false
console.log(b instanceof Object);  // true
```

如果你能在一个对象（数组也是一种对象）的原型链上能够找到Array构造函数，那么这个Object就是一个数组

### 3. 用constructor判断

实例化的数组拥有一个construtor属性，这个属性指向生成这个数组的方法

![1531104110308](https://wave-1253805456.cos.ap-chengdu.myqcloud.com/WaveBlog/20180806145200.png)



``` javascript
const a = [];
console.log(a.constructor);  // function Array(){ [native code] }
```

我们可以通过这样的方式来判断

```javascript
const o = {};
console.log(o.constructor);  // function Oject(){ [native code] }
const r = /^[0~9]$/;  
console.log(r.constructor);  // function RegExp(){ [native code] }
const n = null;
console.log(n.constructor);  // Uncaught TypeError: Cannot read property 'constructor' of null
```

但是constructor会被改写，所以这种方法其实也不太靠谱

### 4. 用Object的toString方法来判断

最行之有效的方法就是使用Object.prototype.toString方法来判断，每一个继承自Object的对象都拥有toString方法

**question**: 为什么不直接使用字符串的toString方法转而使用对象Object的toString方法呢？

``` javascript
const a = ["Hello", "Lang"];
const b = { 0: "Hello", 1: "Lang" };
const c = "Hello Lang";
console.log(a.toString());  // "Hello,Lang"
console.log(b.toString());  // "[object Object]"
console.log(c.toString());  // "Hello,Lang"
```

从上面的结果中我们可以看到这里并没有返回其数据类型，而是返回了内容的字符串

我们可以这样来做

```javascript
const a = ["Hello", "Lang"];
const b = { 0: "Hello", 1: "Lang" };
const c = "Hello Lang";
console.log(Object.prototype.toString.call(a));  // "[object Array]"
console.log(Object.prototype.toString.call(b));  // "[object Object]"
console.log(Object.prototype.toString.call(c));  // "[object String]"
```

所以我们就可以通过这样的方式来判断

### 5.Array.isArray() 

最好的方式，ES5中内容，但是IE11才支持。