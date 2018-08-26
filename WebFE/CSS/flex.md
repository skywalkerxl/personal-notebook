---
title: Flex布局
date: 2018-07-26 23:10:12
tags: [CSS]
categories: CSS
---
今天来聊聊Flex布局
<!--more-->
Flex布局是09年提出的布局方案，在不考虑IE浏览器兼容性的情况，该布局方案可以用来替代position float所带来的布局痛点，下面就来掌握和学习Flex布局

### Flex布局的基本语法要点

首先我们需要明白的是，Flex布局有容器（Flex-Container）和项目（Flex-Item）的两个概念，container和Item各有自己的属性，基本属性如下。

#### 容器的属性

1. flex-direction：决定主轴的方向
2. flex-wrap：决定容器内项目是否可换行
3. flex-flow：flex-direction和flex-wrap的简写形式
4. justify-content：定义了项目在主轴的对齐方式
5. align-items：定义了项目在交叉轴上的对齐方式
6. align-content：定义了多根轴线的对齐方式，只有一根轴线时不起作用

#### 项目的属性

1. order：定义项目在容器中的排序
2. flex-basis：定义了项目在分配多余主轴空间之时，项目占据的主轴空间
3. flex-grow：定义项目的放大比例
4. flex-shrink：定义了项目的缩小比例
5. flex：flex-grow,flex-shrink,basis的缩写
6. align-self：允许单个项目与其它项目有不一样的对齐方式

### 参考文献
[30 分钟学会 Flex 布局](https://zhuanlan.zhihu.com/p/25303493?refer=learncoding)
[2017年如何在移动端优雅的使用flex](https://zhuanlan.zhihu.com/p/29637639)