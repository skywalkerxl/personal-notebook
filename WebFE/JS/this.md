# this

关于this，其实就是要搞明白一个问题`这个this到底是什么？`

1. 当this在全局范围内调用时，指的是全局对象，浏览器下指的就是window，node环境下指global
2. 当this在全局函数中调用时，指的也是全局对象
3. 在对象的方法中调用时，指的是当前对象
4. 在用new创建对象时，this指向的是创建的对象
5. 做显示绑定时，指的是传入的对象

有个比较特殊的地方是`箭头函数中的this`

1. 和之前的执行时绑定不同，箭头函数中的this为定义时绑定
2. 箭头函数中的this指的是父级的this



首先要解决的一个问题是找到this是在哪里被调用的，也就是this的调用位置，然后根据this的绑定规则来判断，this的绑定规则有四条，如果有多条绑定规则相互影响的话，就要考虑this绑定规则的优先级，最后则是ES6中的this绑定情况。

### 1. 先来说说this的调用位置

ES6之前的this调用位置是执行时，而ES6中的箭头函数则是定义时绑定。





### 一.、作为对象的方法调用

``` javascript
let obj = {
    a: 1,
    getA: function () {
        console.log( this === obj );  // true, 这里的this就是obj
        console.log( this.a );
    }
};
```



### 二.、作为普通函数调用

``` javascript
window.name = "Lang";
    // 当作为普通函数调用时，this指向的则是全局对象
    let getName = function () {
        console.log(this.name);
        console.log(this);
    };
    getName();  // Lang

    // 或者
    let myObject = {
        name: "MyObject",
        myObjectName: function () {
            console.log(this.name);
        }
    };

    let myObjectName = myObject.myObjectName;
    myObjectName();  // Lang
```

### 三、构造器调用

``` javascript
let MyClass = function () {
        this.name = "MyClass";
    };

    // 当使用new操作符创建对象时，this就会指向被创建的对象
    let obj = new MyClass();
    console.log(obj.name);

    // 如果构造器显式的返回一个object类型的对象，那么构造的结果返回的则是那个对象
    let AnotherClass = function () {
        this.name = "AnotherClass";
        return {  // 这里显式的返回了一个对象，则返回的就是这个对象
            name: "objName"
        }
    };

    let AnotherObj = new AnotherClass();
    console.log(AnotherObj.name);   //objName

    // 如果构造器不显式的返回数据，或者返回一个非对象类型的数据
    /**
     * @return {string}
     */
    let ThirdClass = function () {
        this.name = "ThirdClass";
        return "nameReturn";
    };

    let thirdObj = new ThirdClass();
    console.log(thirdObj.name);  // ThirdClass
```

### 四、丢失的this

``` javascript
document.getElementById("box1").onclick = function () {
        console.log(this.id);
    };

    // 使用call和apply修正document.getElementById
    document.getElementById = (function (fn) {
        return function () {
            return fn.apply(document, arguments);
        }
    })(document.getElementById);

    // 这里的getId是普通函数，里面的this指向 window,而原生方法中很多的this指向的是document,从而导致报错
    // this修正之后
    let getId = document.getElementById;
    getId("box2");

    // 这里就可以使this指向document
    let getIdCorrect = function (id) {
        return document.getElementById(id);
    };

```

