---
title: 后端Node之【Express】
categories:
- Node框架
- Node
tags:
- Node框架
- Node
---

Node 之 Express 框架入门.

<!--more-->



## Node 之 Express 框架入门

- 简单的 Express 服务器
- 静态文件服务
- 路由
- 中间件



#### 1、简单的 Express 服务器

> 在 http 模块的基础上进行开发的。
>



##### 1.1 安装 express

```
npm install express
```



##### 1.2 搭建

```javascript
// 导入模块
var express = require('express')
// 生成实例
var app = express()

// 处理请求
app.get('/', function(req, res){
    res.end('end')
})

// 监听端口号
app.listen(18000, function(){
    console.log('服务器已启动...')
})
```



#### 2、路由

在 express 中是通过路由来对不同请求进行响应的。



##### 2.1 express 项目模板

```
npm install -g express-generator
```



##### 2.2 创建一个新目录，生成模板

```
express 项目模板名
```



##### 2.3 安装模块中的依赖	

```
npm install
```



##### 2.4 运行该项目

```
npm run start
```

```
localhost://3000    //访问端口号 3000
```



#### 3、静态文件服务

- 网页
- 纯文本
- 图片
- 前端 JavaScript 代码
- CSS 样式表文件
- 媒体文件
- 字体文件



##### 3.1 express 创建静态文件服务

```javascript
// 导入模块
var express = require('express')
// 生成实例
var app = express()

// 返回静态文件
app.use(express.static('./public'))

// 监听端口号
app.listen(18000, function(){
    console.log('服务器已启动')
})
```



##### 3.2 直接访问文件名

```
http://localhost:18000/xiaolu.html
```



#### 4、路由

- 将不同的请求，分配给相应的处理函数
- 区分：路径、请求方法



##### 4.1 路由实现的三种方法

- **path（Post、Get）**
- **Router**
- **route**



##### 4.3 Router 方式

**同一路由下的多个子路由。**针对于同一类型请求（post、get）下不同的请求。

```javascript
// 路由实现
var Router = express.Router();

Router.get('/add', function(req, res){
    res.end('Router /add \n')
})
Router.get('/list', function(req, res){
    res.end('Router /list \n')
})

// 加入到配置中
app.use('/post', Router)
```



##### 4.4 rote

针对不同路由（**文件夹**）下的**不同方法**下的请求。

```javascript
app.route('article')
    .get(function(req, res){
        res.end('route /article get/\n')
    })
    .post(function(req, res){
        res.end('route /article post/\n')
    });
```



##### 4.5 路由参数

用户通过 URL 传递过来的参数，用作路由参数来进行请求操作。

```javascript
// 定义路由参数
app.param('newsId', function(req, res, next, newsId){
    req.newsId = newsId;
    next();
})

app.get('/nexs/:newsId', function(req, res){
    res.end('newsID: ' + req.newsId)
})
```



#### 5、中间件

- Connect: Node.js 的中间件框架；
- 分层处理；
- 每层实现一个功能。

如：next() 操作就是一个中间件，会继续执行下一层的功能。