---
title: TCP 三次挥手与四次握手
date: 2018-09-17 10:12:45
tags: [计算机网络]
categories: [计算机网络]
---

TCP 三次握手与四次挥手



### 1. 三次握手



#### 步骤：

1. 首先 `Server` 端处于监听状态；
2. `Client` 向 `Server` 发送连接请求报文，`SYN = 1`，`ACK = 0`；
3. `Server` 收到 `Client` 连接请求报文后，如果同意建立连接，则回复已收到请求连接报文，`SYN = 1`，`ACK = 1`；
4. `Client` 收到 `Server` 的回复报文后，再向 `Server` 发送确认报文；
5. `Server` 收到 `Client` 的确认回复报文后，建立连接；

### 2. 四次挥手



#### 步骤：

1. `Client` 向 `Server` 发送连接释放报文，`FIN = 1`
2. `Server` 接收到报文后发出确认，此时 `Client` 已无法向 `Server` 发送报文
3. 当`Server`端不再需要连接时，则向`Client`发送连接释放报文，`FIN = 1`
4. `Client` 端收到后发出确认，等待 `2 MSL` 后断开链接
5. `Server` 端收到后断开链接



### 参考内容

1. [TCP三次握手与四次挥手](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C.md#tcp-%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B)