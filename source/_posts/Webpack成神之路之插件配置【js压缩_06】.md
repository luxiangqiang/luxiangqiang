---
title: Webpack成神之路之插件配置【js压缩_06】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---

使用插件对 JS 进行压缩！

<!--more-->



## webpack 插件压缩 js

> 在 `webpack` 已经集成，但是有的还是需要手动安装。



#### 1、安装 uglifyjs-webpack-glugin

> `webpack3` 需要安装低版本的 `uglifyjs-webpack-glugin`

```
// 查看版本号
npm view uglifyjs-webpack-plugin versions
// 安装一定版本的 uglifyjs-webpack-glugin
npm install uglifyjs-webpack-plugin@1.3.0 --save-dev
```



#### 2、导入插件

> 在 `webpack.config.js` 中引入 `uglifyjs-webpack-glugi` 插件。
>
> **注意：** `webpack.json` 文件中的 `plugins`  配置的插件一定要引用。

```
const uglify = require('uglifyjs-webpack-plugin');
```



#### 3、配置插件

> `plugins` 配置里 `new` 一个 `uglify` 对象 。
>
> **注意：**不同版本不同的写法。

```javascript
plugins:[
    new uglify()
],
```



#### 4、进行压缩

> `webpack` 进行压缩，你会发现文件的大小前后的变化，文件变小了。









