---
title: Webpack 成神之路【服务和热更新_04】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---

服务和热更新配置！

<!--more-->



## 配置文件：服务和热更新

### 一、设置 webpack-dev-server 

#### 1、下载 **webpack-dev-server** 

```
npm install webpack-dev-server –-save-dev
```



#### 2、配置 devServer

> 在 `webpack.config.js ` 文件中进行配置。

```javascript
devServer:{
    //设置基本目录结构
    contentBase:path.resolve(__dirname,'dist'),
    //服务器的IP地址，可以使用IP也可以使用localhost
    host:'localhost',
    //服务端压缩是否开启
    compress:true,
    //配置服务端口号
    port:1717
}
```

- contentBase:配置服务器基本运行路径，用于找到程序打包地址。
- host：服务运行地址，建议使用本机IP，这里为了讲解方便，所以用localhost。
- compress：服务器端压缩选型，一般设置为开启，如果你对服务器压缩感兴趣，可以自行学习。
- port：服务运行端口，建议不使用80，很容易被占用，这里使用了1717.















