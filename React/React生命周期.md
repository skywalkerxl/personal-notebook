---
title: React生命周期
date: 2018-08-27 16:33:45
tags: [JS, React]
categories: [JS, React]
---

React生命周期  LifeCycle

React生命周期主要包括如下三个阶段和一个错误捕获

### 阶段一、组件挂载  Mount

1. `defaultProps`
2. `constructor()`
3. `componentWillMount()`
4. `render()`  react 在这里生成虚拟 dom 节点
5. `componentDidMount()`  这里放 ajax 请求



### 阶段二、 组件更新 Update

1. `componentWillReceiveProps(nextProps)`  props 改变时触发
2. `shouldCompnentUpdate(nextProps, nextState)` 需要返回 `true` 或者 `false` ，性能优化的地方
3. `componentWillUpdate()` 
4. `render()`  
5. `componentDidUpdate()`



### 阶段三、 组件卸载 UnMount

1. `componentWillUnMount()` 



### 错误捕获

1. `componentDidCatch()`



## 相关问题：

### 1. ajax 请求应该放在哪个生命周期

