---
title: 后端Node之框架【Koa2】
categories:
- Node 框架
- Node
tags:
- Node 框架
- Node
---

Koa2 基础！

<!--more-->



## Koa2 基础

- 安装 koa2
- async 和 await 语法
- koa2 中间件
- koa2 路由
- cookie 和 session
- mongoose 基础
- redis 基础



## 一、安装 koa2 

#### 1、初始化环境

```javascript
npm init -y
```



#### 2、安装

```
npm install -save koa
```



#### 3、开启服务器处理请求

```javascript
const Koa = require('koa')
const app = new Koa()
 
app.use( async ( ctx ) => {
  ctx.body = 'hello koa2'
})
 
app.listen(3000)
console.log('[demo] start-quick is starting at port 3000')
```



#### 4、启动服务器

```javascript
node index.js
```



## 二、async 和 await 语法

之所以使用 async 和 await 语法，是因为为了能够等待得到异步任务的结果，在执行一下代码，如果不使用 await 语法，就会导致异步的执行顺序变乱。



#### 1、什么是 async

> async 是为了让方法编程异步，只要方法前基上 async ，该函数就返回一个 Promise。



#### 2、什么是 await

> await 是在 async 方法中执行的，是为了等待异步方法的返回结果。



### 三、Get 和 Post 请求的接收

> 在 koa2 中的请求参数可以在 request 对象中获取，也可以在上下文对象 ctx 中获取。



#### 1、query和querystring

- query：返回的是格式化好的参数对象。
- querystring：返回的是请求字符串。

```javascript
const Koa = require('koa');
const app = new Koa();
app.use(async(ctx)=>{
    let url =ctx.url;
    let request =ctx.request;
    let req_query = request.query;
    let req_querystring = request.querystring;
 
    ctx.body={
        url,
        req_query,
        req_querystring
    }
 
});
 
app.listen(3000,()=>{
    console.log('[demo] server is starting at port 3000');
});
```

进行请求：

```
http://127.0.0.1:3000/?user=xiaolu&age=21
```

运行结果输出一下内容：

```json
{
    "url": "/?user=xiaolu&age=21",
    "req_query": {
        "user": "xiaolu",
        "age": "21"
    },
    "req_querystring": "user=xiaolu&age=21"
}
```



#### 2、Post 请求

- 解析上下文 ctx 中的原生 nodex.js 对象 req。
- 将 POST 表单数据解析成 query string-字符串。
- 将字符串转换成 JSON 格式。



**2.1 ctx.request 和ctx.req**区别？

- `ctx.request`:是 Koa2 中 context 经过封装的请求对象，它用起来更直观和简单。
- `tx.req`: 是 context 提供的 node.js 原生 HTTP 请求对象。这个虽然不那么直观，但是可以得到更多的内容，适合我们深度编程。



**2.2 ctx.method**可以直接得到请求类型。



#### 3、接受 Post 请求

- **异步**接收到 post 请求数据拼接为字符串

```javascript
function parsePostData(ctx){
    return new Promise((resolve, reject)=>{
        try{
            let postdata = "";
            // 监听数据(原生的监听事件)
            ctx.req.addListener('data',(data)=>{
                postdata += data;
            })
            // Koa 的监听完成事件
            ctx.req.on('end',()=>{
                resolve(postdata)
            })
        }catch(error){
            reject(error)
        }
    })
```



- 将字符串进行解析为 ` object` 对象形式

```javascript
function parseQueryStr(queryStr){
    let queryData={};
    let queryStrList = queryStr.split('&');
    console.log(queryStrList);
    for( let [index,queryStr] of queryStrList.entries() ){
        let itemList = queryStr.split('=');
        console.log(itemList);
        queryData[itemList[0]] = decodeURIComponent(itemList[1]);
    } 
    return queryData
}
```



```javascript
const Koa = require('koa');
const app = new Koa();
app.use(async(ctx)=>{
     //当请求时GET请求时，显示表单让用户填写
     if(ctx.url==='/' && ctx.method === 'GET'){
        let html =`
            <h1>Koa2 request post demo</h1>
            <form method="POST"  action="/">
                <p>userName</p>
                <input name="userName" /> <br/>
                <p>age</p>
                <input name="age" /> <br/>
                <p>webSite</p>
                <input name='webSite' /><br/>
                <button type="submit">submit</button>
            </form>
        `;
        ctx.body =html;
    //当请求时POST请求时
    }else if(ctx.url==='/' && ctx.method === 'POST'){
        let postData = await parsePostData(ctx);
        ctx.body = postData;
    }else{
        //其它请求显示404页面
        ctx.body='<h1>404!</h1>';
    }
});

function parsePostData(ctx){
    return new Promise((resolve, reject)=>{
        try{
            let postdata = "";
            // 监听数据(原生的监听事件)
            ctx.req.addListener('data',(data)=>{
                postdata += data;
            })
            // Koa 的监听完成事件
            ctx.req.on('end',()=>{
                resolve(postdata)
            })
        }catch(error){
            reject(error)
        }
    })
}

app.listen(3000,()=>{
    console.log('[demo] server is starting at port 3000');
})
```



### 四、koa-bodyparser中间件

#### 4.1  什么是中间件？

> `koa-bodyparser `中间件可以把 `koa2`上下文的 `formData `数据解析到 `ctx.request.body `中。



#### 4.2 安装中间件

```javascript
npm install --save koa-bodyparser@3
```



#### 4.3 引入使用

安装完成后，需要在代码中引入并使用。我们在代码顶部用 `require ` 进行引入。

```javascript
const bodyParser = require('koa-bodyparser');
```



#### 4.4 中间件直接将数据返回到了 ctx.request.body 中

```javascript
const Koa  = require('koa');
const app = new Koa();
// 导入中间件
const bodyParser = require('koa-bodyparser');
 // 使用中间件
app.use(bodyParser());

app.use(async(ctx)=>{
    if(ctx.url==='/' && ctx.method==='GET'){
        //显示表单页面
        let html=`
            <h1>JSPang Koa2 request POST</h1>
            <form method="POST" action="/">
                <p>userName</p>
                <input name="userName" /><br/>
                <p>age</p>
                <input name="age" /><br/>
                <p>website</p>
                <input name="webSite" /><br/>
                <button type="submit">submit</button>
            </form>
        `;
        ctx.body=html;
    }else if(ctx.url==='/' && ctx.method==='POST'){
        let postData= ctx.request.body;
        ctx.body=postData;
    }else{
        ctx.body='<h1>404!</h1>';
    }
 
});
 
app.listen(3000,()=>{
    console.log('[demo] server is starting at port 3000');
});
```

```javascript
{
    "userName": "admin",
    "age": "21",
    "webSite": "luxaignqiang.com"
}
```



### 五、Koa2 原生路由

路由的实现是为了能够更好的区分跳转页面，更好的展现页面效果。



#### 5.1 获取地址栏的URL

要想判断页面，首先拿到地址栏的 URL ，它的 URL 就存在上下文 ctx 中的 request 中 的 URL。

```javascript
const Koa = require('koa')
const app = new Koa()
 
app.use( async ( ctx ) => {
  let url = ctx.request.url
  ctx.body = url
})
app.listen(3000)
```

```javascript
// 访问 3000 得到URL
localhost:3000/admin //得到的结果为 /admin
```



#### 5.2 原生路由实现

```javascript
const Koa = require('koa');
const fs = require('fs');
const app = new Koa();
 
function render(page){
    return new Promise((resolve,reject)=>{
        let pageUrl = `./page/${page}`;
        fs.readFile(pageUrl,"binary",(err,data)=>{
            console.log(444);
            if(err){
                reject(err)
            }else{
                resolve(data);
            }
        })
    })
}
 
async function route(url){
    let page = '404.html';
    switch(url){
        case '/':
            page ='index.html';
            break;
        case '/index':
            page ='index.html';
            break;
        case '/todo':
            page = 'todo.html';
            break;
        case '/404':
            page = '404.html';
            break;
        default:
            break; 
    }
    let html = await render(page);
    
    return html;
}
 
app.use(async(ctx)=>{
    let url = ctx.request.url;
    let html = await route(url);
    
    ctx.body=html;
})
app.listen(3000);
console.log('starting at 3000');
```



### 六、Koa-router 中间件（1）入门

#### 6.1 安装 koa-router

```javascript
npm install --save koa-router
```



#### 6.2 基本使用

```javascript
const Koa = require('koa');
const Router = require('koa-router');
 
const app = new Koa();
const router = new Router();

// 实现多个路由配置
router
    .get('/', function (ctx, next) {
        ctx.body="Hello JSPang";
    })
    .get('/todo',(ctx,next)=>{
        ctx.body="Todo page"
    });
 
app
  .use(router.routes()) // 装载路由
  .use(router.allowedMethods()); // 并确定请求方法，否则报错


app.listen(3000,()=>{
    console.log('starting at port 3000');
});
```







































































































