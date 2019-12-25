---
title: 后端Node之【项目组织和管理】
categories:
- 后端
- Node
tags:
- 后端
- Node
---

Node 项目组织和管理.

<!--more-->

## Node 项目组织和管理

- 项目的初始化、文件结构和模块管理
- 后端项目实践
- 简单的 CMS （内容管理系统）



### 一、项目的初始化、文件结构和模块管理

- 项目的文件结构
- 使用工具来完成初始化
- 模块的组织和管理



#### 1、项目的文件结构

- 水平组织结构 ： 每个所有类型的文件放到一块 MVC（一般用于服务端）
- 垂直组织结构 ：每个功能单独放在一个文件夹（一般用于客户端）



#### 2、项目初始化

- 前端初始化：规范 package.json 文件
- 后台初始化：规范 bower.json 文件



#### 3、模块化

- 遵照 MVC
- 使用 module.exports 和 exports



### 二、后端项目实践

#### 1、npm 初始化项目与Express 项目的结构

##### 1.1 初始化项目（配置文件）

> 创建 package.json 文件。

```javascript
npm init 
```

入口文件设置为 `bin/www`



##### 2.2 创建项目目录 

- config : 存放配置文件(文件夹)
  - config.js : 根据系统相应的环境变量来读取写好的配置文件
  - env : 环境变量目录(文件夹)
    - `development.js : `开发环境的配置文件(文件)
- bin：入口文件
  - www: 入口执行文件
- app : 存放后台文件



##### 2.3 配置环境变量下的开发环境(config/env/development.js )

> 开发环境的配置文件

```javascript
// 开发环境的配置文件
module.exports = {
    port: 7101  // 端口号
}
```



##### 2.4 在 config 目录下创建配置配置文件 config.js 

> 根据系统相应的环境变量来读取写好的配置文件，比如：development.js 。

```javascript
var config = null;

// 判断系统环境变量是否存在
if(process && process.env && process.env.NODE_ENV){
    config = require('./env/' + process.env.NODE_ENV + '.js');
}else{
    config = require('./env/development.js');
}

// 暴露接口，导出
module.exports = config;
```



##### 2.5 创建 express 文件（config/express.js）

> 创建服务器的实例，导出该实例，在其他地方可以直接使用。

设置中间件，返回各种错误（404、500）

```javascript
var express = require('express');       // express 框架
var bodyParser = require('body-parser');// 解析 post 中的数据

module.exports = function () {
    console.log('express 初始化...')
    var app = express();

    // 使用 express 的中间件，app.use() 方法
    app.use();

    // 设置 404 响应
    app.use(function (req, res, next) {
        res.status(404);
        try {
            return res.json('Not Found');
        } catch (e) {
            console.error('404 set header after sent')
        }
    });

    // 来处理各种错误
    app.use(function (err, req, res, next) {
        if (!err) {
            return next();
        }
        res.status(500)
        try{
            return res.json(err.message || 'server error')
        }catch(e){
            console.error('500 set header after sent')
        }
    })

    // 返回实例
    return app;
}
```



##### 2.6 在项目的根目录下创建 app.js 文件

> 该文件主要引入配置文件，进行实例化。

```javascript
var express = require('./config/express')
var app = express();

// 将 app 实例导出，在 bin/www 文件目录中使用
module.exports = app;
```



##### 2.7 创建入口文件（bin/www）

> 负责引入所有的配置文件，进行初始化操作，同时启动服务器，监听端口号。

```javascript
#!/usr/bin/env node  

var app = require('../app')
var config = require('../config/config')

// 监听系统端口
app.listen(config.port,function(){
    console.log('入口系统已启动，正在监听中...... port: ', config.port)
})
```



##### 2.8 安装 nodemon 

> 用来监视 node.js 应用程序中的任何更改并自动重启服务,非常适合用在开发环境中。

```
npm install -g nodemon
```

启动服务器

```
nodemon bin/www
```



### 三、简单的 CMS （内容管理系统）

#### 1、采用水平目录结构

> 在 app 文件夹下创建文件夹，controllers、models、router 三个文件夹。

`app`

- controllers
- models
  - news.server.model.js
- router:

`config`

- mongoose.js



#### 2、创建各个文件

##### 2.1 新建后台新闻的 mongodb 模型

```javascript
// 创建 MongoDB 的模型
var mongoose = require('mongoose');

// 设计集合结构
var NewsSchema = new mongoose.Schema({
    titles: String,
    content: String,
    createTime: {
        type: Date,
        default: Date.now
    }
})

// 创建该模型
var News = mongoose.model('News',NewsSchema)
```



##### 2.2 创建并配置 mongoose 配置文件

- 导入环境配置参数
- 初始化配置 mongoose 模型

```javascript
// mongoose 配置文件 —— 连接数据库
var mongoose = require('mongoose')
var config = require('./config') // 导入环境配置参数

module.exports = function(){
    // 连接数据库
    var db = mongoose.connect(config.mongodb)

    // 初始化配置 mongoose 模型
    require('../app/models/news.server.model')

    return db;
}
```



##### 2.3 在 app 文件生成数据库的实例

```javascript
var express = require('./config/express')
var mongodb = require('./config/mongoose')

var db = mongodb();
var app = express();

// 将 app 实例导出，在 bin/www 文件目录中使用
module.exports = app;
```



## 初始化前端项目

### 一、bower 完成前端初始化

> Bower 是一个客户端技术的软件包管理器，它可用于搜索、安装和卸载如 JavaScript、HTML、CSS 之类的网络资源。其他一些建立在 Bower 基础之上的开发工具，如 YeoMan 和 Grunt，这个会在以后的文章中介绍。



#### 1、安装 bower 

```
npm install -g bower
```

#### 2、初始化项目

```
bower init
```

#### 3、在 express 加入静态文件

```
// 加入静态文件
app.use(express.static("./public"))
```

#### 4、设置将安装的第三方库放在 lib 目录

在根目录新建 .bowerrc 文件，加入以下配置。

```
{
    "directory": "public/lib"
}
```

#### 5、安装第三方插件（Vue、Bootstrap、jQuery）











