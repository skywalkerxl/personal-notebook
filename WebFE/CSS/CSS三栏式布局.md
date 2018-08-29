---
title: CSS三栏式布局
date: 2018-08-28 11:28:03
tags: [CSS]
categories: [CSS]
---

# CSS三栏式布局

CSS三栏式布局是前端比较常见的布局，有【上中下式】和【左中右式】布局，现以PC端为布局基准

## 一、左中右式布局

### 1. 首先是通过定位来实现布局

``` html
<div class="left"></div>
<div class="main"></div>
<div class="right"></div>
```

``` css
html, body {
    margin: 0; padding: 0;  // 清除默认样式
}
.left, .right, .main {
    height: 200px; 
}
.left {
    position: absolute;
    left: 0; width: 200px;
    background: palevioletred;
}
.rigth {
    position: absolute;
    rigth: 0; width: 200px;            
    background: paleturquoise;
}
.main {
    margin: 0 200px 0 200px;            
    background: palegoldenrod;
}
```

这样就可以实现我们的三栏样式布局，但是会有一个问题，如果我们给中间栏一个最小宽度的样式，我们就会发现在浏览器宽度缩小时，右栏会遮住中间栏部分

![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS三栏式布局.assets\2018082801.gif)

### 2. 通过浮动也可以实现三栏布局

先上代码

``` html
<body>
	<div class="left"></div>
	<div class="right"></div>
	<div class="main">文本内容文本内容文本内容文本内容</div>
</body>
```

```css
html, body {
    margin: 0; padding: 0;
}
.left {
    float: left;
    width: 200px; height: 200px;
    background: palegoldenrod;
}
.right {
    float: right;
    width: 300px; height: 200px;
    background: paleturquoise;
}
.main {
    margin: 0 300px 0 200px;
    background: palevioletred;
}
```

但是这种方式会有错位的问题

![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS三栏式布局.assets\2018082808.gif)

### 3. 圣杯布局

圣杯布局的思路其实很简单，主要分成以下步骤：

1. 左中右全部浮动
2. 中间内容宽度为100%, 正是由于中间宽度为100%，左右两边的位置被挤下去了，那么我们呢通过设置left和right部分的margin-left属性来让左右块上移
3. 上移之后发现中间部分内容被遮挡，那我们可以通过设置父级元素的padding-left 和padding-right分别为左右部分的宽度，再设置左右部分相对定位来让左右部分分别左右移开

``` html
<body>
    <div class="main"></div> <!-- 这里要设置中间层优先渲染，否则会被遮挡住 -->
    <div class="left"></div>
    <div class="right"></div>
</body>
```

``` css
.left, .right, .main {
    float: left;
    height: 300px;  /* 这里给一个固定高度方便辨认 */
}
.left {
    position: relative;
    left: -100px;　width: 100px; 
    margin-left: -100%;
    background: 
}
.right {
    position: relative;
    right: -100px; width: 100px;
    margin-left: -100px;
    background: 
}
.main {
    width: 100%;
    background: 
}

```

![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS三栏式布局.assets\2018082805.gif)

这里我们发现错位了。。。

### 4. 双飞翼布局

双飞翼布局的思想则是给中间元素多包一层父级元素，从而不用额外给左右两侧添加相对定位

``` html
<div class="main">
    <div class="inner"></div>
</div>
<div class="left"></div>
<div class="right"></div>
```

``` css
body,html {
    height:100%;
    padding: 0;
    margin: 0
}
body {
    /*padding-left:100px;*/
    /*padding-right:200px;*/
}
.left {
    background: paleturquoise;
    width: 100px;
    float: left;
    margin-left: -100%;
    height: 100px;
    /*position: relative;*/
    /*left:-100px;*/
}

.right {
    background: palegoldenrod;
    width: 200px;
    float: left;
    margin-left: -200px;
    height: 100px;
    /*position:relative;*/
    /*right:-200px;*/
}

.main {
    background: palevioletred;
    width: 100%;
    float: left;
    height: 100px;
}

.inner {
    margin-left: 100px;
    margin-right: 200px;
    min-width: 200px;
}
```

![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS三栏式布局.assets\2018082806.gif)

如果我们的底部需要加一个自适应上层高度的footer，那么只需要添加一个父级，并清除浮动就可以了

``` html
<body>
<div class="container clear">
    <div class="main">
        <div class="inner">
            文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容文本内容
        </div>
    </div>
    <div class="left">Left</div>
    <div class="right">Right</div>
</div>
<footer class="footer">footer</footer>
</body>
```

``` css
body, html {
    height:100%;
    padding: 0;
    margin: 0
}
body {
    /*padding-left:100px;*/
    /*padding-right:200px;*/
}
.container {
    position: relative;
}
.clear:after{
    display: block;
    content: "";
    clear: both;
}
.left {
    background: paleturquoise;
    width: 100px;
    float: left;
    margin-left: -100%;
    height: 100px;
    /*position: relative;*/
    /*left:-100px;*/
}

.right {
    background: palegoldenrod;
    width: 200px;
    float: left;
    margin-left: -200px;
    height: 100px;
    /*position:relative;*/
    /*right:-200px;*/
}

.main {
    background: palevioletred;
    width: 100%;
    float: left;
}

.inner {
    margin-left: 100px;
    margin-right: 200px;
    min-width: 200px;
}
.footer {
    height: 100px;
    background: peachpuff;
}
```

我们可以看到如下图所示

![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS三栏式布局.assets\2018082807.gif)

在兼容低版本的浏览器时，双飞翼是个很好的选择，像`display: inline-block`或者`display: table-cell`也是可以实现三栏布局的。

### 5. 面向主流浏览器的flex布局

flex布局轻松惬意

``` html
<body>
<div class="container">
    <div class="left"></div>
    <div class="main">文本内容文本内容文本内容文本内容</div>
    <div class="right"></div>
</div>
<footer class="footer">footer</footer>
</body>
```

``` css
html, body {
    margin: 0 ; padding: 0;
}
.container {
    display: flex;  /* 不需要给子元素设置浮动定位等 */
}
.left {
    height: 200px; 
    width: 200px;
    background: paleturquoise;
}
.main {
    flex: 1;  /* 居中部分只要一个flex就可以了 */
    background: palevioletred;
}
.right {
    height: 200px;
    width: 300px;
    background: palegoldenrod;
}
.footer {
    height: 100px;
    background: papayawhip;
}
```



![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS三栏式布局.assets\2018082809.gif)

###6. 使用grid布局

grid用起来也很轻松惬意

``` html
<body>
<div class="container">
    <div class="left">left</div>
    <div class="main">文本内容文本内容文本内容</div>
    <div class="right">right</div>
</div>
<footer class="footer">footer</footer>
</body>
```

``` css
html, body {
    margin: 0; padding: 0;
}
.container {
    display: grid;
    grid-template-columns: 200px 1fr 400px;
}
.left {
    height: 200px;
    background: palegoldenrod;
}
.right {
    height: 200px;
    background: paleturquoise;
}
.main {
    background: palevioletred;
}
.footer {
    background: papayawhip;
    height: 100px;
}
```

![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS三栏式布局.assets\2018082810.gif)



## 二、上中下式布局

