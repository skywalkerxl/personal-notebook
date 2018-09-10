---
title: 原生JS实现轮播图
date: 2018-09-07 12:35:14
tags: [JS, 组件开发]
categories: [JS, 组件开发]
---

原生 JS 实现无限轮播图

我们直接用 `CSS3` 来实现我们的动画效果

## 最初版实现

先上效果图

![](https://wave-1253805456.cos.ap-chengdu.myqcloud.com/WaveBlog/personal-notebook/WebFE/%E7%BB%84%E4%BB%B6%E5%8C%96%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97/%E8%BD%AE%E6%92%AD%E5%9B%BE%E7%BB%84%E4%BB%B6/2.gif)

`html` 部分

``` html
<div class="container">
    <ul class="wrapper animation" style="left: -400px;">
        <li class="list list5">5</li>
        <li class="list list1">1</li>
        <li class="list list2">2</li>
        <li class="list list3">3</li>
        <li class="list list4">4</li>
        <li class="list list5">5</li>
        <li class="list list1">1</li>
    </ul>
</div>
<button id="btnStart">start</button>
<button id="btnStop">stop</button>
```

`css` 部分

``` css
html, body, ul, li { margin: 0; padding: 0; }
li { list-style: none; }
.container {
    width: 400px; height: 200px;
    margin: 100px auto;
    overflow: hidden;
}
.wrapper {
    position: relative;
    width: 2800px; height: 200px;
}
.wrapper.animation { transition: left .5s linear; }
.wrapper .list {
    float: left;
    width: 400px; height: 200px;
}
.wrapper .list1 { background: papayawhip; }
.wrapper .list2 { background: palegoldenrod; }
.wrapper .list3 { background: palevioletred; }
.wrapper .list4 { background: paleturquoise; }
.wrapper .list5 { background: mediumpurple; }
```

`js` 部分

``` js
(function () {
    let $ul = document.querySelector('ul.wrapper');
    let $btnStop = document.querySelector('#btnStop');  // 暂停按钮
    let $btnStart = document.querySelector('#btnStart');  // 开始按钮
    let timer = null;

    // 【开始】按钮点击时开启定时器
    $btnStart.addEventListener('click', function () {
        timer = setInterval(function () {
            $ul.style.left = parseInt($ul.style.left) - 400 + "px";
            // 当移动到标号为1的滑块，也即倒数第一个 li 时
            // 我们让他回到真正的1号滑块，这样就做到了无缝滑动
            if( $ul.style.left === "-2400px" ){
                setTimeout(function () {
                    $ul.classList.remove('animation');
                    $ul.style.left = "-400px";
                    setTimeout(function () {
                        $ul.classList.add("animation");
                    }, 100);
                }, 600);
            }
        }, 1000);
    }, false);

    // 【结束】按钮点击时清除定时器
    $btnStop.addEventListener('click', function () {
        clearInterval(timer);
    }, false);
})();
```

我们这样可以写出一个轮播图，但是太简陋了，我们既不方便扩展他，也不方便维护他

所以我们来尝试把他写成一个组件的形式，方便我们扩展

我们来研究下上述可以实现的地方有哪些可扩展的地方呢？

* **个数**。我们现在的个数还是写的太固定了，我们可以让他可以自由的扩展
* **可随处使用**。我们的组件只能在这一处使用，其它地方就不可使用了

我们看一下改版后的代码

``` html
<!-- 第一个轮播图 start -->
<!--
  这里我们给我们的 container 写上样式之后就可以自动初始化了
-->
<div id="swiper1" class="myswiper-container">
    <ul class="myswiper-wrapper animation">
        <li class="myswiper-list list1">1</li>
        <li class="myswiper-list list2">2</li>
        <li class="myswiper-list list3">3</li>
        <li class="myswiper-list list4">4</li>
        <li class="myswiper-list list5">5</li>
    </ul>
</div>
<button id="btnStart_1">start</button>
<button id="btnStop_1">stop</button>
<!-- 第一个轮播图 end && 第二个轮播图 start -->
<div id="swiper_2" class="myswiper-container">
    <ul class="myswiper-wrapper animation">
        <li class="myswiper-list list1">1</li>
        <li class="myswiper-list list2">2</li>
        <li class="myswiper-list list3">3</li>
        <li class="myswiper-list list4">4</li>
        <li class="myswiper-list list5">5</li>
    </ul>
</div>
<button id="btnStart_2">start</button>
<button id="btnStop_2">stop</button>
<!-- 第二个轮播图 end -->
```

初始化代码

``` javascript
let s1 = new MySwiper({
    container: '#swiper1',
    btnStop: '#btnStop_1',
    btnStart: '#btnStart_1'
});

let s2 = new MySwiper({
    container: '#swiper_2',
    btnStop: '#btnStop_2',
    btnStart: '#btnStart_2'
});

s1.init();
s2.init();
```

插件部分代码

``` javascript
+(function () {
    let MySwiper = function(options){
        this.settings = { };
        for(let key in options){
            if(options.hasOwnProperty(key)){
                this.settings[key] = options[key] || this.settings[key];
            }
        }
    };

    MySwiper.prototype = {
        // 这里获取我们的组件
        g: function(el){
            return document.querySelector(el);
        },
        // 这里绑定事件
        on: function(el, type, fn){
            el.addEventListener(type, fn);
        },
        // 这里初始化我们的组件
        init: function () {
            let self = this;
            let $container = self.g(self.settings["container"]);
            let $wrapper = self.g(`${self.settings["container"]} ul.myswiper-wrapper`);
            let $btnStop = self.g(self.settings["btnStop"]);
            let $btnStart = self.g(self.settings["btnStart"]);
            let timer = null;

            // 初始化 wrapper 内 html 结构
            let newLastNode = $wrapper.querySelector(':last-child').cloneNode(true);
            let newFirstNode = $wrapper.querySelector(':first-child').cloneNode(true);

            // 前后各添加一个节点
            $wrapper.appendChild(newFirstNode);
            $wrapper.insertBefore(newLastNode, $wrapper.querySelector(':first-child'));

            // 初始化父级 style
            let childWidth = $wrapper.querySelector(':first-child').clientWidth;
            $wrapper.style.left = `-${childWidth}px`;

            self.on($btnStart, 'click', function () {
                timer = setInterval(function () {
                    $wrapper.style.left = parseInt($wrapper.style.left) - childWidth + "px";
                    if( $wrapper.style.left === `-${childWidth * 6}px` ){
                        setTimeout(function () {
                            $wrapper.classList.remove('animation');
                            $wrapper.style.left = `-${childWidth}px`;
                            setTimeout(function () {
                                $wrapper.classList.add("animation");
                            }, 100);
                        }, 600)
                    }
                }, 1000);
            }, false);

            self.on($btnStop, 'click', function () {
                clearInterval(timer);
            }, false);
        }
    };

    window["MySwiper"] = MySwiper;
})();
```

效果如下

![](https://wave-1253805456.cos.ap-chengdu.myqcloud.com/WaveBlog/personal-notebook/WebFE/%E7%BB%84%E4%BB%B6%E5%8C%96%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97/%E8%BD%AE%E6%92%AD%E5%9B%BE%E7%BB%84%E4%BB%B6/3.gif)



### 可扩展部分

1. 使用 `transform` 来替代使用 `left` 等，减少浏览器 `repaint`
2. 可以开启 `GPU加速` 来加速渲染我们的页面
3. 可以使用 `requestAnimation` 来平滑我们的定时器动画
4. 组件编写方式还可以再扩展