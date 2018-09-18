---
title: JS事件委托
date: 2018-09-05 13:56:09
tags: [JS, 事件委托]
categories: [JS]
---

JS事件委托

事件委托就是把子元素的事件冒泡到父元素来处理

例如，我们把列表元素中 li 的点击事件委托到 父级ul 的事件来处理

``` html
<ul id="list"></ul>
```

HTML 结构如上

JS 代码如下

``` javascript
let $ul = document.getElementById('list');

// 这里通过 document.createDocumentFragment() 创建文档碎片
let listFacg = document.createDocumentFragment();
for(let i = 0 ; i < 10 ; i ++){
    let $li = document.createElement('li');
    let text = document.createTextNode(`测试${i + 1}`);
    $li.appendChild(text);
    listFacg.appendChild($li);
}

$ul.append(listFacg);

// 冒泡到父级绑定事件
$ul.addEventListener('click', function(event){
    if(event.currentTarget === $ul){
        console.log(event.target);
    }
}, false);
```



