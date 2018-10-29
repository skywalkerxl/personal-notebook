---
date: 2018-10-29 09:59:14
title: 理解Buffer
tags: [NodeJS]
categories: [NodeJS]
---

`Buffer` 可以理解为原`JavaScript` 中对 `string` 操作的扩展部分

## 一、Buffer 的结构

### 1. 模块结构

![1540783327418](assets/1540783327418.png)

从这张图里，我们可以看到 `Buffer` 本身分成两个部分：

1. `C++`，`Buffer` 的性能相关实现部分用 `C++ `来实现
2. `JavaScript`，`Buffer` 的非性能相关实现部分用 `JavaScript`来实现

### 2.Buffer 对象



### 3. Buffer 内存分配

由于Buffer一般用于大文件或数据流的处理部分，Buffer的内存分配并不是在V8的堆内存中，而是在C++层面的来申请的内存

#### slab机制

slab简单来说就是一块申请好的大小固定的内存区域

这块内存区域有三个状态，如下：

1. `full`：完全分配状态
2. `partial`：部分分配状态
3. `empty`：没有被分配状态

#### 分配小的Buffer对象

采用 `slab` 机制进行预先申请和事后分配

#### 分配大的Buffer对象

大块的`Buffer`，直接使用 `C++ `层面的调用



## 二、Buffer的转换

### 1. Buffer 支持的对象类型

* ASCII
* UTF-8
* UTF-16LE/UCS-2
* Base64
* Binary
* Hex

### 2. 字符串转Buffer

字符串转`Buffer` 主要通过构造函数的方式完成

### 3. Buffer转字符串

通过`Buffer`对象的`toString()`来完成



## 三、Buffer 的拼接

一般情况下，可以通过字符串拼接的形式来拼接Buffer对象

但是由于中文在Buffer中，占三个位置，这就导致了乱码的问题



### 1. setEncoding() 与 string_decoder() 

通过给可读流设置编码的方法 `setEncoding` 来解决编码的问题



### 2. 正确拼接Buffer

调用 `Buffer.concat()` 方法来生成一个合并的 Buffer 对象



## 四、Buffer与性能

通过预先转换静态内容为Buffer对象，可以有效的减少CPU的重复使用，节省服务器资源