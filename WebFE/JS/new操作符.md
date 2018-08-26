new运算符 的作用

1. 首先创建一个新的空对象 `var obj = {}`
2. 其次会将构造函数的原型赋值到新对象的原型上，也即`obj.__proto__ = F.prototype`
3. 绑定this到新创建的对象上，也即`F.call(obj)`

我们可以通过这样的方式来实现一个new运算符

``` javascript
MyObject.newObject = function(){
    var obj = {};
    var myObject = MyObject;
    obj.__proto__ = myObject.prototype; // 赋值原型
    myObject.call(obj, arguments);
    return obj;
}
```

