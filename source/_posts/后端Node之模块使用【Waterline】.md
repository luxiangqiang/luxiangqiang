---
title: 后端Node之模块【Waterline】
categories:
- Node 模块
- Node
tags:
- Node 模块
- Node
---

后端 Node 之模块 Waterline 操作数据库。

<!--more-->



## 目录

[TOC]

## Node.js 的 ORM 模块 Waterline 

- Wateline 的基本介绍
- Waterline 中的主要概念



#### 1、Wateline 的基本介绍

- **ORM 的基本概念**
- **特点与优势**
- **与 mongoose 对比**



##### 1.1 ORM 的基本概念

- ORM（Object Relational Mapping）对象映射关系。比如：hibernate 就是一种 ORM 框架，Node 中也有 ORM 框架，分为大型用的轻量级和中小型用的。Waterline 就是 node 其中一种 ORM 框架。
- 将文档数据库中的一个文档，关系数据库表中的一行，映射为 JavaScript 中的一个对象。
- 操作对象，便可完成对数据库的操作。



##### 1.2  waterline 特点与优势

- 支持大部分的主流数据库
- 脱离 SQL
- 使用同样的代码操作不同的数据库（填写不同的配置）
- 易于理解的符号
- 丰富的方法（操作数据库）
- 多样的数据类型（细化数据类型支持）



##### 1.3 与 mongoose 对比

- 支持更多的数据库
- 提供比 Mongoose 更丰富的 CURD 方法
- 更丰富的数据校验方法



#### 2、Waterline 中的主要概念

- 适配器
- 连接
- 数据集合
- 校验器
- 生命周期回调



##### 2.1 适配器

waterline 支持不同的数据库，不同的数据库配置通过适配器进行配置。

将统一的操作代码，转化为某种数据库所支持的数据库操作。

不同适配器是不同的单独的模块。



##### 2.2 连接

通过某个适配器，及对应的连接信息，来建立一个与数据库的实际连接。



##### 2.3 数据集合

- 定义具体的数据类型 
- 类似 Mongoose 中的 Model 
- 具体对应关系数据库中的表，和文档数据库中的集合 



##### 2.4 校验器

- 执行数据检查
- 使用的是第三方模块 Anchor（Github:https://github.com/sails/anchor）
- 预定义的数据校验器，支持常规检查、时间检查等
- 支持自定义的数据校验



##### 2.5 生命周期回调

- 创建时：beforeVaildate / afterVaildate / beforeCreate / afterCreate 
- 更新时：beforeVaildate / afterVaildate / beforeUpdate /afterUpdate 
- 删除时：beforeDestroy / afterDestory 





## 使用 http 模块创建 Web 服务器

#### 一、基本概念

- 创建一个 web 服务器
  - 接受 HTTP 请求
  - 处理 HTTP 请求
  - 做出响应
- 常见的 Web 服务器架构
  - Nginx / Apache：负责接受 HTTP 请求，确定谁来处理请求，并返回请求的结果
  - php-fpm / php 模块：处理分配给自己的请求，并将处理的结果返回给分配者

- 常见的请求种类
  - 请求文件：静态文件（网页、图片、css文件）
  - 完成特定的操作：如登录、特定的数据等。

#### 二、Node.js 的 Web 服务器

- 不依赖其他的 Web 服务器软件（如：Apache、Nginx、lls......）
- 可以直接处理请求的逻辑
- Node.js 代码负责 Web 服务器的各种“配置”（没有复杂的环境配置） —— 代码



#### 三、创建一个服务器

> 借助 Node.js 的核心模块来创建一个简单的服务器。

Node 模块分为两种：

- 核心模块：node 自带的模块
- 第三方模块

```javascript
var http = require('http')

// 设置监听请求的回调函(会传入两个参数)
var requestHandle = function(req, res){
    res.end('hello')
}

// 创建一个服务器
var web = http.createServer(requestHandle)
// 设置服务器的监听端口
web.listen(18000)

console.log('服务器已经启动...')
```






## 创建 TCP（Socket） 服务器 — TCP 协议

- 使用 net 模块创建 TCP 服务
- 使用 telnet 连接 TCP 服务器



#### 1、使用 net 模块创建 TCP 服务

Node 的核心模块 net 创建 TCP 服务器。

```javascript
var net = require('net')

const PORT = 18001;
const HOST = '127.0.0.1'

var clientHandle = function(socket){
    console.log('有人来连接了')

    // 监听 socket 事件
    socket.on('data', function dataHandler(data){
        console.log('adress:' + socket.remoteAddress, socket.remotePort,'send:', data)
    })

    // 连接断开监听事件
    socket.on('close', function(){
        console.log(socket.remoteAddress, socket.remotePort, '断开连接！')
    })
}

var app = net.createServer(clientHandle)

// 监听端口
app.listen(PORT,HOST)
console.log('服务器已启动...')
```



#### 2、使用 telnet 连接 TCP 服务器

HTTP Web 服务器使用浏览器来测试，而 TCP 服务器使用 telnet 来进行测试。

```
npm install telnet
```

```
telnet localhost 18001
```



#### 3、使用 net 创建 TCP 连接

```javascript
var net = require('net')

var tcpClient = net.Socket();

tcpClient.connect(18001, '127.0.0.1', function(){
    console.log('连接成功！')
    tcpClient.write('这是一个TCP 连接')
})

tcpClient.on('data', function(data){
    console.log('data', data)
})
```



## Node.js 异步优化

- Node.js 异步优化简介
- Node.js 优化异步代码
- Node.js 异步优化性能对比



### 一、Node.js 异步优化简介

#### 1、基本概念

- 同步概念 — 阻塞式调用，等待执行（按照顺序执行）。
- 回调概念 —回调是一个异步等效的功能（在回调函数中执行回调结果）。
- 异步概念 — 非阻塞，通知获取结果。

### 

#### 2、异步优化的目的

##### 2.1 异步代码潜在的问题

- 难以阅读
- 编码困难
- 调试复杂



### 二、Node.js 优化异步代码

#### 1、异步优化库介绍

- callback 方法

  异步函数封装原则

- 调用类库

  > 在callback进行封装

  Step

  Asunc

  Then.js



#### 2、CO 库的介绍





#### 3、CO 库异步优化实践













