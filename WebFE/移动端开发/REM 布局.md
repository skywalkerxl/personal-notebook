---
title: REM 布局
date: 2018-09-19 08:15:49
tags: [移动端开发]
categories: [移动端开发]
---

REM 布局设置

因为 REM 对应的是根元素的字体大小，我们可以这样做

在元素的头部部分添加 script 标签更改根元素的字体大小

``` javascript
let html = document.documentElement;
let hWidth = html.getBoundingClientRect().width;  // 获取页面宽度
html.style.fontSize = hWidth / 8 + 'px';
```

这里我们设置了 `1 rem` 的大小为页面宽度的 `1/8`

我们如果想让个元素宽度为页面宽度的 `1/4`

我们可以将元素的宽度设置为

``` css
.box { width: 2rem; }  /* 设置为两个rem之后，宽度即为所求 */
```

如果用sass的话，可以使用如下的方程式

``` scss
$ue-width: 750;

@function px2rem($px){
    @return #{$px/$ue-width*100}rem
}
```

