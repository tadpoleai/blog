title: 如何设计 API 接口
author: 'ADoyle <adoyle.h@gmail.com>'
tags: ["SOA", "API"]
categories: []
copyright: '未经授权，不得全文转载。转载前请先阅读[本站版权声明](http://adoyle.me/blog/copyright.html)'
---

## /前言(Intro)


<!-- more -->


## 思考
一个Api服务在正常的情况下，会返回正确的响应，但是有很多情况可能造成服务无法正确处理，如：

请求的参数不合法
服务资源不可用
权限或安全原因不可访问 等等
由于客户端只能根据服务端返回的报文，知道究竟发生了什么问题，因此在发生任何异常时，服务端都必须返回一个客户端可识别错误信息，这样，客户端才能进行相应的处理。

## 异步 or 同步

> 除了 GET 操作，POST\PUT\DELETE 是否都可以不必等待服务端完成逻辑，直接返回成功？


## 有状态 or 无状态

## 错误与异常

### 错误类型
服务需要报告的错误实际上可归入两类：
#### 系统级别
表示属于服务实现的一部分的运行时软件组件、硬件和网络通信协议的错误。这些错误代表不与所执行的业务逻辑或数据相关的错误。
例如，如果服务实现需要访问的 RDBMS 服务器崩溃，由于应用程序服务器无法找到该服务器的 Java™ Database Connectivity (JDBC) 连接，因此您的服务器无法处理请求消息，则将出现典型的系统级错误。
#### 业务级别
由于服务违反业务逻辑或数据相关规则而引发的错误。例如，如果请求消息表示请求预订旅程，而回程日期字段的日期早于出发日期，则服务实现将可能引发业务级别的错误。


#### 可恢复性错误
可恢复性错误是指那些可通过恰当的可选执行路径来恢复用户程序的错误。这些错误是由于不遵守某一特定业务规则所导致的。


所谓业务错误实际上是一种可恢复性出错。一旦服务组合进入了最后的定稿阶段，那么请从以下几个错误处理的方面来评估可重用服务：

- 业务错误场景——详细描述会标记某业务操作无效的条件。（上下文）
- 错误概述——当某一业务错误发生时服务消费者收到的业务出错提示，它提供对业务出错的简单描述。（error message）
- 错误代码——可用于查询和某错误相关的附加信息的代码。(error code)
- 建议——给服务消费者的反馈，诸如有效输入、或显示有关出错的具体信息等等。
- 服务区域——标识某一服务区域，该区域可接收到与服务系统错误相关的所有通知

#### 不可恢复性错误
这些错误是客户程序无法恢复的。此类错误是在运行时中由某些不可预期的错误产生的，诸如空指针、资源不可用等编程性错误。

### 错误恢复
事务回滚和补偿事务是解决问题的两种方法。


## 参考(Bibliographies)
- [][B1]

## 引用(References)
[^1]: [][R1]


<!-- 以下是相关链接 -->

[R1]: <url> "备注"

[B1]: <url> "备注"