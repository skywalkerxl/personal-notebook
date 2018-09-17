---
title: XSS 和 CSRF 攻击
date: 2018-09-17 08:44:24
tags: [Web安全]
categories: [Web安全]
---

XSS 和 CSRF 攻击

### 1. XSS 攻击

跨站脚本攻击

#### 攻击形式

主要通过注入恶意的客户端代码，

#### 如何防范

1. 添加HttpOnly 放置窃取cookie
2. 客户端用户输入检查
3. 服务端输出检查



### 2. CSRF 攻击

跨站请求伪造攻击

#### 攻击形式

盗用用户的cookie来骗取服务器的信任，从而发送恶意请求等来达到攻击目的

#### 如何防范

1. 验证码
2. HTTP 头部字段 Referer 验证
3. 添加 token