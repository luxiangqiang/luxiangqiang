---
title: Webpack成神之路之插件配置【HTML发布_07】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---

打包发布 HTML 文件！

<!--more-->



### 一、开发环境和生产环境

> 在实际开发中，`webpack` 配置文件是分开的，开发环境一个文件，生产环境一个文件。
>
> **注意：两者设置冲突会报错！**

- **开发环境：**开发环境中是基本不会对 `js` 进行压缩的，在开发预览时需要明确的报错行数和错误信息，所以完全没有必要压缩 `avasScript` 代码。热更新的插件 `devServer` 用于开发环境 。
- **生产环境：**生产环境中才会压缩 `JS` 代码，用于加快程序的工作效率。



### 二、打包 HTML 文件

#### 1、安装打包插件

> `webpack` 安装时 `html-webpack-plugin` 注意版本号。
>
> **注意：**我这里用的是 webpack3 插件的版本号为 2.30.1。

```javascript
npm install --save-dev html-webpack-plugin@2.30.1
```



#### 2、webpack.config.js文件引入html-webpack-plugin插件。 

```javascript
const htmlPlugin= require('html-webpack-plugin');
```



#### 3、webpack.config.js 里的 plugins 里进行插件配置 

```javascript
 new htmlPlugin({
     minify:{
         removeAttributeQuotes:true
     },
     hash:true,
     template:'./src/index.html'
 })
```

- **minify：**是对 `html` 文件进行压缩，`removeAttrubuteQuotes` 是却掉属性的双引号。
- **hash：**为了开发中 `js` 有缓存效果，所以加入 `hash`，这样可以有效避免缓存 `JS`。
- **template： **是要打包的 `html` 模版路径和文件名称。



#### 4、webpack 进行打包

> `HTML` 文件里引用的 `js` 路径可以去掉，打包之后它会自动加上。