在JS里面，`call`、`apply`、`bind`用于改变this的指向，他们之间有着一些相同点，也有着一些区别

### 1. apply和call的区别

`call`和`apply`同为改变this的指向，区别在于两者的参数不同，

`call`的第一个参数为this要绑定的对象，之后的参数为要传入的参数

`apply`的第一个参数同样也为要绑定的对象，第二个参数为一个参数数组

也就是说，如果你知道要传入多少个参数的话，用call比较好，也可以说call就是apply的一个语法糖



### 2. bind和apply、call的区别

bind与其它方法不同的是，他会返回一个新的函数，这个函数中的this指向的则是新的对象,必须要手动调用

``` javascript
let obj1 = {
    a: 0,
    b: 0,
    add: function(c){
        return this.a + this.b + c;
    }
}

let obj2 = {
    a: 1,
    b: 2
}

obj1.add();  // NaN
obj1.add.bind(obj2, 3);  // function (c){ return this.a + this.b + c }
obj1.add.bind(obj2, 3)(); // 6
obj1.add.call(obj2, 3); // 6
obj1.add.apply(obj2, [3]);  // 6
```

## 如何实现bind函数

### 1. bind函数中只传入一个参数，也即要绑定的对象

``` javascript
Function.prototype.bind2 = function(context){
    var self = this; // 这里的self保存的是调用该方法的function
    return function(){
        // 这里如果不用self，而用this的话，调用是全局对象window
        self.apply(context, arguments);
    }
}
```



### 2. 如果bind函数中，我们还要传入其他参数我们可以这样做

``` javascript
Function.prototype.bind = function(){
    var self = this,
        context = [].shift.call(arguments),
        args = [].slice.call(arguments);
    
    return function(){
        self.apply( context, [].concat.call( args, [].slice.call(arguments) ) );
    }
}
```

