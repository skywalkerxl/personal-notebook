---
title: BFC
date: 2018-09-12 09:42:57
tags: [BFC]
categories: [BFC]
---

BFC   `Block Formatting Context`

简单来说就是CSS布局的一种规则



###  布局规则1：内部的Box会在垂直方向上从上往下放置



### 布局规则2：BFC 内相邻 Box 的 margin 会重叠

例如我们有如下 html 部分

``` html
<div class="top">top</div>
<div class="bottom">bottom</div>
```

CSS 部分如下

``` css
.top { width: 300px; height: 300px; background: palegoldenrod; margin-bottom: 100px; }
.bottom { width: 300px; height: 300px; background: palevioletred; margin-top: 130px; }
```

呈现效果如下

![1536717619544](D:\MyRepository\personal-notebook\WebFE\CSS\BFC.assets\1536717619544.png)

我们在这里会发现，上下两个box重叠了

这里我们抛出一个问题，如果我们的盒子是 `inline-box` 会发生纵向 margin 重叠吗?

如下图所示，并不会发生重叠。

![1536717884108](D:\MyRepository\personal-notebook\WebFE\CSS\BFC.assets\1536717884108.png)

那回到刚才的话题，如何阻止这种相邻块级元素在同一个 BFC 中产生纵向 margin 重叠的问题呢？

解决方式是：

1. 首先我们把这两个相邻元素分割到不同的 BFC 当中
2. 其次我们再触发父级的 BFC （那么问题来了，如何触发一个元素的 BFC ？）

首先在 html 中我们给 top 块包裹一个 wrapper 块，html 结构如下

``` html
<div class="wrapper">
    <div class="top">top</div>
</div>
<div class="bottom">bottom</div>
```

同样，我们需要触发父级的 BFC，CSS代码如下

``` css
.wrapper {
    overflow: hidden;  /* 这里我们通过触发 overflow: hidden 来触发父级元素额BFC */
}
.top {
    width: 300px;
    height: 200px;
    background: palegoldenrod;
    margin-bottom: 100px;
}
.bottom {
    width: 300px;
    height: 200px;
    background: palevioletred;
    margin-top: 130px;
}
```



![1536718526043](D:\MyRepository\personal-notebook\WebFE\CSS\BFC.assets\1536718526043.png)

### 布局规则3：子元素的 margin-box 会与 父级元素的 border-box 接触

如下图所示，我们给 top 和 bottom 设置了浮动，我们可以看到尽管我们没有设置父级清浮动，但是我们仍然可以看到父级与子元素内部的边距

![1536720040027](D:\MyRepository\personal-notebook\WebFE\CSS\BFC.assets\1536720040027.png)

既然说到了浮动，那我们就通过 BFC 这块来解释下清浮动，其实讲白了也很简单，就是触发父级 BFC

我们分别使用 `display: inline-block` 、`overflow: hidden`、`position: absolute`等，都能实现我们的清浮动作用

![1536720677874](D:\MyRepository\personal-notebook\WebFE\CSS\BFC.assets\1536720677874.png)



### 布局规则4：触发了 BFC 的元素不会与 浮动元素重叠

先看下面这个例子

![](D:\MyRepository\personal-notebook\WebFE\CSS\格式化上下文-BFC.assets\1.gif)

那我们可以看到 right 元素在不触发 BFC 即正常情况下会与 left 浮动元素重叠

增加了 BFC 之后，我们的 right 元素就不再与左边的浮动元素重叠

