---
title: Flex布局
date: 2018-07-26 23:10:12
tags: [CSS]
categories: CSS
---
今天来聊聊Flex布局
<!--more-->
Flex布局是09年提出的布局方案，在不考虑IE浏览器兼容性的情况，该布局方案可以用来替代position float所带来的布局痛点，下面就来掌握和学习Flex布局

## 一、Flex布局的基本语法要点

首先我们需要明白的是，Flex布局有容器（Flex-Container）和项目（Flex-Item）的两个概念，container和Item各有自己的属性，基本属性如下。

### 1. 容器的属性

1. flex-direction：决定主轴的方向
2. flex-wrap：决定容器内项目是否可换行
3. flex-flow：flex-direction和flex-wrap的简写形式
4. justify-content：定义了项目在主轴的对齐方式
5. align-items：定义了项目在交叉轴上的对齐方式
6. align-content：定义了多根轴线的对齐方式，只有一根轴线时不起作用

### 2. 项目的属性

1. order：定义项目在容器中的排序
2. flex-basis：定义了项目在分配多余主轴空间之时，项目占据的主轴空间
3. flex-grow：定义项目的放大比例
4. flex-shrink：定义了项目的缩小比例
5. flex：flex-grow,flex-shrink,basis的缩写
6. align-self：允许单个项目与其它项目有不一样的对齐方式

## 二、 从案例入手

### 1. flex-direction  flex-wrap  flex-flow

我们设置元素横排不换行,可以这样做

``` CSS
.container { flex-direction: row; flex-wrap: nowrap; }
/* 简写形式 */
.container { flex-flow: row nowrap }
```

效果如下：

![](D:\MyRepository\personal-notebook\WebFE\CSS\flex.assets\3.gif)

我们希望元素可以自动换行时，我们可以这样做

``` css
.container { flex-direction: row; flex-wrap: wrap; }
/* 简写形式 */
.container { flex-flow: row nowrap; }
```

![](D:\MyRepository\personal-notebook\WebFE\CSS\flex.assets\4.gif)

有时候我们会有些奇怪的需求，比如说多出的元素换行到上面

``` css
.container {
    flex-direction: row;
    flex-wrap: wrap-reverse;  /* 设置 -reverse 即可 */
}
/* 简写形式 */
.container { flex-flow: row wrap-reverse; }
```

![](D:\MyRepository\personal-notebook\WebFE\CSS\flex.assets\5.gif)



### 2. justify-content 对齐方式

对方方式除了我们常用的 居左，居右，居中，对齐之外，还有个边距对齐

对应的分别是：

1. `flex-start`
2. `flex-end`
3. `center`
4. `space-between`
5. `space-around`

效果如下：

![1536732673295](D:\MyRepository\personal-notebook\WebFE\CSS\flex.assets\1536732673295.png)



### 3. align-items 对齐方式

除了我们常说的 顶部，底部，居中，拉伸之外，还有个基线对齐

对应的分别是：

1. `flex-start`
2. `flex-end`
3. `center`
4. `strench(默认)`
5. `baseline` 基线对齐

效果如下：

这里注意一点，如果说这里的 strech 如果你设置了高度，那么拉伸是无效的

![1536733677197](D:\MyRepository\personal-notebook\WebFE\CSS\flex.assets\1536733677197.png)



### 4. align-content 多行内容的对齐方式

这里其实和 `justify-content` 对齐方式差不多

只不过 `justify-content` 对应的是主轴对应着单个子元素

而 `align-content` 对应着交叉轴上的行而已

偷懒截的图：

![1536734506378](D:\MyRepository\personal-notebook\WebFE\CSS\flex.assets\1536734506378.png)

## 三、常见案例分析

### 1. 实现一个三栏布局

``` css
.container {
    display: flex;
}
.items:nth-of-type(1) {
    width: 200px;
}
.items:nth-of-type(2) {
    flex: 1;
}
.items:nth-of-type(3) {
    width: 200px;
}
```





### 2. 实现一个响应式的三栏布局	

![](D:\MyRepository\personal-notebook\WebFE\CSS\flex.assets\1.gif)

``` css
/* css 部分代码如下 */
.container {
    display: flex;
}

/* 大屏 */
@media all and (min-width: 1200px){
    .container {
        justify-contents : flex-end;
    }
    .items {
        width: 150px; height: 40px;
        text-align: center; line-height: 40px; 
    }
}

/* 中屏 */
@media all and (min-width: 992px) and (max-width: 1199){
    .container {
        justify-contents : space-between;
    }
    .items {
        width: 150px; height: 40px;
        text-align: center; line-height: 40px; 
    }
}

/* 小屏 */
@media all and (max-width: 768px){
    .container {
        flex-direction: column;
        align-items: center;
    }
    .items {
        width: 150px; height: 40px;
        text-align: center; line-height: 40px; 
    }
}
```

## 四、浏览器的兼容性



### 参考文献

[30 分钟学会 Flex 布局](https://zhuanlan.zhihu.com/p/25303493?refer=learncoding)
[2017年如何在移动端优雅的使用flex](https://zhuanlan.zhihu.com/p/29637639)
[flex布局完全指南](https://zhuanlan.zhihu.com/p/25984121)

