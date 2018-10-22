---
date: 2018-10-20 15:23:44
title： mysql部分数据库操作需要注意的点
tags: [database, mysql]
categories: [database]
---

### 1. 默认时间问题

Mysql 5.7以上的版本设置默认时间时，不要设置 '0000-00-00 00:00:00'

使用最小时间：`1970-01-01 08:00:01`

参考这篇文章

[[MySQL timestamp的默认值怎么设置？](https://segmentfault.com/q/1010000007933803)]

