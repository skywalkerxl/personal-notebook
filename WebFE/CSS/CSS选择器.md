---
title: CSS 选择器
date：2018-09-10 14:13:26
tags: [CSS]
categories: [CSS]
---

CSS 选择器

### 1.  基本选择器

兼容性： `全部兼容`

| 选择器               | 含义       | 举例              |
| -------------------- | ---------- | ----------------- |
| *                    | 通配选择   | `* { }`           |
| #id                  | ID选择器   | `#content { }`    |
| E                    | 元素选择器 | `html { }`        |
| .className           | 类选择器   | `.container{ }`   |
| selector1, selector2 | 群组选择器 | `html, body { } ` |



### 2.  层次选择器

兼容性：`IE7 +`

| 选择器 | 含义           | 举例 |
| ------ | -------------- | ---- |
| E F    | 后代选择器     |      |
| E > F  | 子选择器       |      |
| E + F  | 相邻兄弟选择器 |      |
| E ~ F  | 通用选择器     |      |

这个地方可能会有些迷惑，举个例子来说明如下：

``` html
<ol class="container">
    <li>测试1</li>
    <li class="content">测试2</li>
    <li>
        <ol>
            <li>子项目1</li>
            <li>子项目2</li>
            <li>子项目3</li>
            <li>子项目4</li>
        </ol>
    </li>
    <li>测试4</li>
</ol>
```

我们通过设置如上不同的 CSS 来看看具体的效果

#### 后代选择器

代码如下：

``` css
ol li { background: peachpuff }
```

效果如下：

![1536651406662](D:\MyRepository\personal-notebook\WebFE\CSS\CSS选择器.assets\1536651406662.png)

#### 子选择器

代码如下：

``` css
.container > li {
    list-style: none;
    background: peachpuff;
}
```

效果如下：

![1536651443733](D:\MyRepository\personal-notebook\WebFE\CSS\CSS选择器.assets\1536651443733.png)



#### 相邻兄弟选择器

会选择自己的相邻节点（下一个节点），而不是前后都会被选择

代码如下：

``` CSS
.content + li {
    list-style: none;
    background: peachpuff;
}
```

效果如下：

![1536651535403](D:\MyRepository\personal-notebook\WebFE\CSS\CSS选择器.assets\1536651535403.png)

#### 通用选择器

代码如下：

``` CSS
.content ~ li {
    list-style: none;
    background: peachpuff;
}
```

效果如下：

![1536651607132](D:\MyRepository\personal-notebook\WebFE\CSS\CSS选择器.assets\1536651607132.png)

### 3. 伪类选择器

伪类选择器包含的条目比较多，可分为如下六种：

1. 动态伪类选择器 `IE 8+` 
2. 目标伪类选择器 `IE 9+`
3. 语言伪类选择器 `IE 8+`
4. UI元素状态伪类选择器 `IE 9+`
5. 结构伪类选择器 `IE 9+`
6. 否定伪类选择器 `IE 9+`

#### 动态伪类选择器

1. `:link`
2. `:visited`
3. `:hover`
4. `:active`
5. `:focus`

#### 目标伪类选择器

1. `:target`

这个伪类可能用的比较少，但是很有用

我们用它来做一个手风琴的例子，先上效果图

![](D:\MyRepository\personal-notebook\WebFE\CSS\CSS选择器.assets\1.gif)

``` html
<div class="container">
    <ul>
        <li id="list1">
            <h1 class="title"><a href="#list1">标题一</a></h1>
            <div class="content">内容一</div>
        </li>
        <li id="list2">
            <h1 class="title"><a href="#list2">标题二</a></h1>
            <div class="content">内容二</div>
        </li>
        <li id="list3">
            <h1 class="title"><a href="#list3">标题三</a></h1>
            <div class="content">内容三</div>
        </li>
    </ul>
</div>
```

``` CSS
.container li{
    height: 40px;
    overflow: hidden;
}
.container .title {
    margin: 0;
    background-color: paleturquoise;
    cursor: pointer;
}
.container .content {
    margin: 0;
    height: 100px;
    background-color: palevioletred;
}

/* 这里的 target 目标也就是我们点击链接时激活的目标，我们在这里伸展其高度，即可得所需效果 */
li:target  {
    height: 140px;
}
```

#### 语言伪类选择器

1. `:lang` 

比如说我们分别编写 原因为中文和英文的例子

``` CSS
:lang(en) { }
:lang(zh) { }
```

#### UI元素状态选择器

1. `:checked`  选中
2. `:enabled`  启用
3. `:disabled`  不可用

#### 结构伪类选择器

根元素，我们目前的 HTML 环境下，等同于 html

1. `:root`

指定子元素部分

1. `:first-child`
2. `:last-child`
3. `:nth-child(n)`
4. `:nth-last-child(n)`

指定类型部分

1. `:first-of-type`
2. `:last-of-type`
3. `:nth-of-type(n)`
4. `:nth-last-of-type(n)`



1. `:only-child`  只包含一个元素
2. `:only-of-type`  只包含一个同类型的元素
3. `:empty` 没有子元素的元素

#### 否定伪类选择器

1. `:not()`

### 4. 伪元素

1. `::first-letter`  选择的第一个字母
2. `::first-line`  选择的第一行
3. `::before`
4. `::after`
5. `::selection` 目前只能更改选择元素的背景 `background` 和 `color`



### 5. 属性选择器

属性选择器这一部分主要类似于属性的正则匹配





## 二、选择器权重与优先级

最高为 `!important` 

第一级：`style` 内敛样式，权重为 `1000`

第二级：`id选择器`，权重为 `0100`

第三级：`类选择器、伪类选择器、属性选择器`，权重为 `0010`

第四级：`标签选择器 p div `,`伪元素选择器 ::after ::before 等`，权重为 `0001`






