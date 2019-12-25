---
title: Webpack 成神之路【多入口多出口_03】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---



webpack 配置文件实现多入口多出口！

<!--more-->



### 一、新建 webpack.config.js 文件

> `webpack.config.js` 这是 `webpack` 自有的配置文件。在根目录新建 `webpack.config.js` 配置文件,属性如下：

```javascript
module.exports={
    //入口文件的配置项
    entry:{},
    //出口文件的配置项
    output:{},
    //模块：例如解读 CSS,图片如何转换，压缩
    module:{},
    //插件，用于生产模版和各项功能
    plugins:[],
    //配置 webpack 开发服务功能
    devServer:{}
}
```

```javascript
const path = require('path')

module.exports={
    entry:{
        // 打包文件的路径
        entry: ''
    },
    output:{
        // 打包路径(获取项目的绝对路径)
        path: path.resolve(__dirname,'dist'),
        //打包的文件名称
        filename: ''
    },
    module:{},
    plugins:[],
    devServer:{}
}
```



### 二、配置属性信息

> 添加入口文件和出口文件进行打包。

```javascript
//双入口配置文件
entry:{
    // 第一个入口文件
    entry:'./src/entry.js',
    //另一个入口文件
    entry2:'./src/entry2.js'
},
```

> 出口文件修改 [name]。

```javascript
//全局
const path = require('path');
//局部
 output:{
     //打包的路径文职，绝对路径
     path:path.resolve(__dirname,'dist'),
     //打包文件名（[name] 根据入口文件的名称作为出口文件名称）
     filename:'[name].js'
 },
```



### 三、命令打包

> 控制台输入 `Webpack`命令进行打包。



### 四、全部代码

```javascript
//node 知识
const path = require('path');
// 配置文件
module.exports={
    //入口文件的配置项
    entry:{
        entry:'./src/entry.js',
        //另一个入口文件
        entry2:'./src/entry2.js'
    },
    //出口文件的配置项
    output:{
        //打包的路径文职，绝对路径
        path:path.resolve(__dirname,'dist'),
        //打包文件名(name)表示对应的入口文件名
        filename:'[name].js'
    },
    //模块：例如解读CSS,图片如何转换，压缩
    module:{},
    //插件，用于生产模版和各项功能
    plugins:[],
    //配置webpack开发服务功能
    devServer:{}
}

```

