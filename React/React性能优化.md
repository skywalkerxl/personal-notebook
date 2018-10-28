---
date: 2018-10-28 10:13:55
title: React 性能优化
tags: [React, 性能优化]
categories: [React]
---

React 性能优化部分

### 1. 性能检测

`Perf.start()`

`Perf.stop()`

`Perf.printWasted()`

### 2. PureRenderMixin 优化

``` javascript
import React from 'react';
import PureRenderMixin from 'react-addons-pure-render-mixin';

class List extends React.Component {
    constructor(props, context){
        super(props, context);
        // 这里替换后，减少无效的组件状态更新
        this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
    }
    // others code
}
```

### 3. immutable.js 优化

适用场景为数据结构层次很高的时候，例如 `obj.a.b.c.d = 10`这种