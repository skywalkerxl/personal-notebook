---
title: CSS 实现超出文本隐藏
date: 2018-09-21 09:20:06
tags: [CSS]
categories: [CSS]
---

CSS 实现超出文本隐藏

单行文本溢出隐藏处理的CSS样式如下

``` CSS
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
```

![1537494794933](D:\MyRepository\personal-notebook\Algorithm\排序\assets\1537494794933.png)



如果是多行文本的话，我们可以这样做：

``` css
/* 我们先给文本定义行高和最大高度 */
.box {
    width: 100%;  /* 这里宽度定到父级宽度 */
    line-height: 20px;  
    max-height: 60px;
    
    position: relative;
    word-wrap: word-break;
    overflow: hidden;
}

.box::after {
    content: '...';
    position: absolute;
    bottom: 0; right: 0;
    padding-left: 40px;
    /* 不加渐变的话，直接加背景颜色也是可以的 */
    background: -moz-linear-gradient(left, transparent, #fff 50%);
    background: -webkit-linear-gradient(left ,transparent, #fff 50%);
    background: -webkit-linear-gradient(left ,transparent, #fff 50%);
    background: -webkit-linear-gradient(left ,transparent, #fff 50%);
}
```



效果如下

![1](D:\MyRepository\personal-notebook\Algorithm\排序\assets\1.gif)

