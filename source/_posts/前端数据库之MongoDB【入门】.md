---
title: 前端数据库之MongoDB【入门及Mongoose模块】
categories:
- 数据库
- MongDB
tags:
- 数据库
- MongDB
---

MongDB 的安装、基本增删改查的命令以及 mongoose 模块操作 MongoDB。

<!--more-->



## MongoDB

- MongoDB 是一种非关系型的数据库，也是一种 “NoSQL” 数据库。
- MongoDB是一个基于分布式文件存储的数据库。
- 在关系型的数据库中有着 “表” 和 “行” 的概念，每个表都是一种数据结构。而 MongoDB 呢，没有表和行的概念。但是对应的是 "集合" 和 “文档”。



## 目录

[TOC]



### 一、基本使用

#### 1、特点

- 使用 BSON 存储数据：BSON 是一种二进制的 JSON，相对于 JSON 在空间和性能上有一定的优势。
- 支持相对丰富的查询操作：以键值对为中心，其他的 NoSQL 操作查询相对较差。
- 支持索引：优化
- 副本集：支持多个服务器运行一个数据库。
- 分片：数据量大，分到不同的服务器，实现服务器的
- 无模式：每个文档的数据结构都是不相同的，所以数据之间互不相关，这就是无模式
- 部署简单方便：注意，涉及到安全问题，MongoDB 下载加载文件就能使用，但是需要配置验证用户使用，不然任何人都会访问到你的数据库。



#### 2、创建 MongoDB

使用之前必须创建 MongoDB 的存储目录（创建在根目录下，且不能低于 4 个 G）。**每个数据库的启动都是 MongoDB 的一个实例。**

##### 2.1 指定数据目录（启动mongoDB）

```
mongod --dbpath=data/db --port=27017 // 指定目录和端口号
```

##### 2.2 关闭 mongoDB

关闭后台 mongodb 服务。

```
mongoDB --shutdown
```

##### 2.3 指定日志目录

为了检查错误和系统的日志分开，便于查看。

```
mongod --dbpath=data/db --port=27017 --fork --log=/data/log/mongd.log
```



#### 3、MongoDB 的 shell 使用

mongodb 没有专门创建数据库的语句，当你真正的使用了一个数据库，并插入一条语句的时候，就创建了。

##### 3.1 先指定数据库的目录，然后连接数据库。

```
// 启动数据库
mongod --dbpath D:\data\db --port=27017
// 连接数据库
mongo   
```

设置用户名和密码连接：

```

```

##### 3.2 查看已经存在的数据库

```
show dbs
```

##### 3.3 显示当前数据库的对象或集合

```
db
```

##### 3.4 使用 use 连接到一个数据库

```
use admin
```

##### 4.4 插入数据

```
db.admin.insert({"name":"小鹿"})
```

##### 5.5 查询

```
db.admin.find()// 集合查询
db.admin.find().count() //返回集合的数量
db.admin.find(“name”:"小鹿")// 条件查询
```

##### 5.6 更新数据

`update` 三个参数：

- 指定修改的数据
- 要修改的值
- `multi` 是否满足全部修改

```
// 默认更改一条数据
db.admin.update({“name”:"小鹿"},{$set{"name":"小明"}});
// 修改多行数据(满足条件的都被修改)
db.admin.update({“name”:"小鹿"},{$set{"name":"小明"}},{multi:true});
```

`save` 方法照样可以修改，但是必须传入ID(**注意：**修改后的存在的字段只有修改的字段)

- Id 和 修改的值

```
db.admin.save({"_id":ObjectId("55sdfdfasd34dsf44tgb"),"age":70})
```

##### 5.7 删除语句

`remove` 参数：

- 删除条件
- 是否指定一行删除

```
db.admin.remove({}) // 删除集合中所有的数据
db.admin.remove({“name”:"小明"}) // 删除所有满足条件的数据
db.admin.remove({“name”:"小明"},true) // 删除满足条件的第一条数据
```

##### 5.8 删除集合

```
db.admin.drop()
```



#### 4、使用 Mongooes 模块操作 MongoDB

有些开发中使用到的文档结构不是无模式的，所有使用第三方模块 mongooes ，既可以用到强关系，也可以使用到强类型。

##### 4.1 什么是 Mongooes 模块？

将 `Node.js` 对象与 `MongoDB` 的文档对应起来的模块。

##### 4.2 安装 Mongooes 模块

```
npm install mongoose --save
```

##### 4.3 在 Node 中导入 mongooes

```javascript
var mongooes = require('mongooes') //导入模块

var uri = 'mongodb://username:password@hostname:port/databasename'
uri = "mongooes://localhost/part1"

// 连接 mongooes
mongooes.connect(uri)
```

##### 4.4 Model 与 Schema

- `Model：` 主要建立 `JS` 对象与 `Mongooes` 对应的文档关系。通过 `Model` 可以对文档进行增、删、改、查。
- `Schema：` 主要对 Model 中**数据类型的定义**，使其在 MongoDB 无模式中实现模式化的存储。

##### 4.5 创建 Schema 和 Model 

```javascript
// 定义 Model 的数据类型
var BookSchema = new mongooes.Schema({
    name: String,
    author: String,
    publishTime: Date
})
mongooes.model('Book', BookSchema)
```

##### 4.6  插入数据（insert.js）

```javascript
var mongoose = require('mongoose');
// 引入模块(执行)
require('./model.js')

// 生成 Model
var Book = mongoose.model('Book')

// 创建 Model 的实例
var book = new Book({
    name: 'JavaScript 高级程序设计',
    author: '阮一峰',
    publishTime: new Date()
});

book.author = '小鹿'

// 保存进数据库
book.save(function(err){
    console.log('save status:', err ? 'failed' : 'success')
});
```

##### 4.7 查找数据

```javascript
var mongoose = require('mongoose');
// 引入模块(执行)
require('./model.js')

var Book = mongoose.model('Book');

// 返回满足数据的所有数据
Book.find({},function(err, docs){
    if(err){
        console.log('err',err)
        return;
    }
    console.log(docs)
})
```

```javascript
var mongoose = require('mongoose')

require('./model.js')

var Book = mongoose.model('Book');

// 返回满足条件的第一条数据
Book.findOne({author:'小鹿'}, function(err, docs){
    if(err){
        console.log('err',err)
        return;
    }
    console.log(docs)
})
```

```javascript
var mongoose = require('mongoose')

require('./model.js')

var Book = mongoose.model('Book');

// 满足条件查询
// or: 满足其中一个条件就会查出
// and: 同时满足两个条件就会查出
var cond = {
    $or: [
        {author: 'Jame'},
        {author: 'Jim'},
    ]
}

// 返回满足数据的所有数据
Book.find(cond,function(err, docs){
    if(err){
        console.log('err',err)
        return;
    }
    console.log(docs)
})
```

##### 4.8 删除数据

```
var mongoose = require('mongoose')

require('./model.js')

var Book = mongoose.model('Book');

// 返回满足条件的第一条数据
Book.findOne({author:'小鹿'}, function(err, docs){
    if(err){
        console.log('err',err)
        return;
    }
    if(docs){
        docs.remove();
    }
})
```



#### 5、在 Express 项目中使用 Mongoose 

##### 5.1 安装 Express 和 Express-generator

> 区别：express 只是一个框架，是在被引入项目中使用的，而非工具。而 express-generator 则是一个工具，此工具的作用是生成express项目。

```
npm install express
```

```
npm install express-generator -g
```

##### 5.2 新建配置文件（config/config.js）

配置连接数据库等相关信息并以模块的形式导出。

```javascript
// 存放与配置文件相关的文件 —— 连接数据等相关信息
module.exports = {
    mongodb: "mongdb://localhost/part1" // 连接数据库的 uri
}
```

##### 5.3 新建 mongoose 配置文件（config/mongoose.js）

用来连接数据库，并以模块形式返回连接数据库的实例。

```javascript
// 配置 mongoose 配置文件 —— 连接数据库

var mongoose = require('mongoose');
var config = require('./config.js');

module.exports = function(){
    // 连接数据逯
    var db = mongoose.connect(config.mongodb) 
    // 返回实例
    return db; 
}
```

##### 5.4 创建 Moudel 与 Schema

新建文件夹moudels，新建文件 user.server.moudel.js 命名为了能够与客户端框架代码区分开。

```javascript
var mongoose = require('mongoose')

// 定义 Schema 数据类型结构
var UserSchema = mongoose.Schema({
    uid: Number,
    usernmame:String,
    createTime: Date,
    loastLogin: Date
})

// 生成模型 Model
mongoose.model('User',UserSchema)
```

##### 5.5 将 model 引入到配置文件中

```javascript
// 配置 mongoose 配置文件 —— 连接数据库

var mongoose = require('mongoose');
var config = require('./config.js');

module.exports = function(){
    // 连接数据库
    var db = mongoose.connect(config.mongodb) 

    // 加载 Model
    require('../moudels/user.server.moudel.js')

    // 返回实例
    return db; 
}
```

##### 5.6 在 app.js 中配置启动时自动加载

```javascript
// 加载 mongose 配置文件
var mongoose = require('./config/mongoose.js')
// 执行配置文件
db = mongoose();
```

##### 5.7 安装 express 依赖的模块

不要忘了单独安装 mongoose 模块。

```
npm install
```

##### 5.8 启动服务器

启动服务器前要启动数据库。

```
node ./bin/www
```

##### 5.9 访问服务器

```
// 打开浏览器访问 3000 端口
locahost:3000
```

##### 5.10 访问数据库

在 router 的 users 中定义一个路由地址，并插入数据，查询数据。

```javascript
// 引入 mongoose
var mongoose = require('mongoose')

// 引入 Model
var User = mongoose.model('User');


// 写上访问路由(请求对象、响应对象、继续执行)
router.get('/test', function(req, res, next){
  // 存储数据
  var user = new User({
    uid: 1,
    username: 'Sid'
  })
  // 执行存储
  user.save(function(err){
    if(err){
      res.end('Error')
      return next();
    }

    User.find({},function(err, docs){
      if(err){
        res.end('Error')
        return next();
      }

      res.json(docs)
    })
  })
})

```

##### 5.11 启动服务器

访问路由，看路由挂载到哪个路径下进行访问。

```
访问 http://localhost:3000/users/test
```



### 二、Mongoose 使用进阶

#### 1、模式的扩展

##### 1.1 默认值

当基于的模型初始化的时候，不给它的值，默认的值。

- 固定值
- 即时生成

##### 1.2 固定值

```javascript
var mongoose = require('mongoose')
// 连接数据库
mongoose.connect('mongodb://localhost/part2')
// 定义数据类型
var UserSchema = new mongoose.Schema({
    nickname: {
        type: String,
        default: 'new user'   //默认盒子
    }
});
// 导入模型
var User = mongoose.model('User',UserSchema)

// 创建实例
var user = new User();
// 打印
console.log('user:', user)
```

##### 1.3 即时生成

```javascript
time: {
    type: Date,
    default: Date.now
}
```

##### 1.4 模式修饰符

> 数据库存储数据或者取出数据的时候进行数据设置设置。

- 预定义的模式修饰符：大写、小写
- 自定义 setter 修饰符
- 自定义 getter 修饰符

##### 1.5 预定义修饰符

```javascript
var mongoose = require('mongoose')

mongoose.connect('mongodb://localhost/part2')

var User = mongoose.model('User', {
    nickName: {
        type: String,
        trim: true   // 去掉空格
    }
}) 

var user = new User({
    nickName: "  Sid  ",
})

console.log('User', user)
```

##### 1.6 setter 

```javascript
 blog: {
     type: String,
         set: function(url){   // 判断输入的值，然后回调函数进行处理
             if(!url) return;

             if(url.indexOf('http://') !== 0 && url.indexOf('https://') !== 0){
                 url = 'http://' + url;
             }

             return url;
         }
 }
```

##### 1.7 getter

```javascript
var mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/part2')
var User = mongoose.model('User', {
    blog: {
        type: String,
        get: function(url){
            if(!url) return url;

            if(url.indexOf('http://') !== 0 && url.indexOf('https://') !== 0){
                url = 'http://' + url;
            }

            return url;
        }
    }
})

var user = new User({
    blog: 'xiaolu.com'
})

user.save(function(err){
    if(err){
        return console.log(err)
    }

    console.log('block ' + user.blog)
})
```

##### 1.8 虚拟属性

设置一个不真实存在的值，用来存储计算属性的结果。

```javascript
var mongoose = require('mongoose')

var PersonSchema = new mongoose.Schema({
    fristName: String,
    lastName: String
})

// 设置虚拟属性
PersonSchema.virtual('fullName').get(function(){
    return this.fristName + ' ' + this.lastName;
})

var Person = mongoose.model('Person', PersonSchema)

var person = new Person({
    fristName: 'Sid',
    lastName: 'Chen'
})

console.log('user fullNmae', person.fullName)
```

注意：模式实例转 Json 一般是不包含虚拟属性的。

```javascript
console.log('JSON: ' + JSON.stringify(person))
```

改一下配置：

```
// 设置当转 JSON 时，虚拟属性也转换
PersonSchema.set('toJSON', {getters: true, virtual: true})
```

##### 1.9 索引

- 唯一索引：是否唯一
- 辅助索引：增加查询速度



#### 2、模型的方法

- 静态方法
- 实例方法

##### 2.1 静态方法

静态方法一般用于数据的查询。

```javascript
var mongoose  = require('mongoose')

mongoose.connect('mongodb://localhost/part2', { useNewUrlParser: true });

// Schema 数据结构
var BookSchema = new mongoose.Schema({
    name: String,
    isbn: Number
})

// 静态方法一般做查询辅助
BookSchema.statics.findByISBN = function(isbn, cb){
    this.findOne({isbn: isbn}, function(err, doc){
        cb(err, doc)
    })
}

// 创建模型
var Book = mongoose.model('Book', BookSchema)
// 生成实例 —— 存储数据 
var book = new Book({
    name: 'MEAN Web Development',
    isbn: 9787100
});

book.save(function(err){
    if(err){
        return console.log('save Book failed', err)
    }

    // 调用静态方法
    Book.findByISBN(9787100, function(err, doc){
        console.log('findByISBN err: doc', err, doc)
    })
})
```

##### 2.2 实例方法

查询是异步的。

```javascript
// 实例方法
BookSchema.methods.print = function(){
    console.log('Book Information');
    console.log('\t Title:', this.name);
    console.log('\t ISBN:', this.isbn);
}

 // 调用实例方法
book.print()
```



#### 3、数据校验

对将存储的数据进行校验，如果符号设定的格式才会存储，否则报错。

##### 3.1 验证必须字段

```javascript
// 数据校验
var OrderSchema = new mongoose.Schema({
    count: {
        type:Number,
        required: true
    }
})
```

##### 3.2 验证最大值和最小值

```javascript
// 数据校验
var OrderSchema = new mongoose.Schema({
    count: {
        type:Number,
        required: true,
        max: 1000,
        min:10
    }
})
```

##### 3.3 验证是否是枚举类型的值

```javascript
status: {
    type: String,
    enum: ['create', 'success', 'failed']
},
```

##### 3.4 验证正则匹配

```javascript
desc: {
    type: String,
    match: /book/g
}
```

##### 3.5 自定义验证器

加一个 `validate` 属性。

```javascript
desc: {
    type: String,
    match: /book/g,
    validate: function(desc){
        return desc.length < 10;
    } 
}
```



#### 4、中间件

在执行某些特定的代码之前要执行的事情。

##### 4.1 串行操作

```javascript
// 串行的操作

var mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/part2',{ useNewUrlParser: true })
var ResellerSchema = new mongoose.Schema({
    address: String
})

// 后置处理中间件(在保存之后处理)
ResellerSchema.post('save', function(next){
    console.log('保存之后调用的中间件！')
    next();
})

var Reseller = new mongoose.model('Reseller', ResellerSchema)
var reseller = new Reseller();
reseller.address = '人们路'
reseller.save()
```

##### 4.2 并行操作

```javascript
// 并行操作
// 参数一：会函数执行的回调函数
// 参数二: done: 是另外一个需要并行任务的回调函数
ResellerSchema.pre('save', true, function(next, done) {
    console.log('并行中间件已经调用！');
    next();
    done();
})
```

##### 4.3 实际用途

- 保存文档：处理用户最后保存的时间。最后保存预处理中间件，修改字段就是保存的时间。
- 删除文档：删除处理中间件，写一个删除日志进行处理。



#### 5、DBRef

用来实现不同“集合”之间的交叉引用。（联表查询）

- DBRef 定义
- populate()

```javascript
// 联表查询

var mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/part2',{ useNewUrlParser: true })
var User = mongoose.model('User', {
    username: String
})
var News = mongoose.model('News', {
    title: String,
    author: {
        type: mongoose.Schema.ObjectId,
        ref: 'User'
    }
})

var user = new User({
    username: 'Sid'
})

var news = new News({
    title: '今日新闻',
    author: user
})

// 先保存用户
user.save(function(err){
    if(err){
        return console.log('save user failed:', err)
    }

    news.save(function(err){
        if(err){
            return console.log('save news failes', err)
        }
        
        // 查询
        News.findOne().populate('author').exec(function(err, doc){
           return console.log('查询结构：', err, doc)
        })
    })
})
```













