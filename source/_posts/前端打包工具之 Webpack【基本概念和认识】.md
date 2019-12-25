---
title: 前端打包工具之 Webpack【01基本认识】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---

前端打包工具 Webpack 的基本认识。

<!--more-->

## 目录

[TOC]





### 一、安装和使用

#### 1、安装 webpack 和 webpack-cli 

```javascript 
npm install webpack webpack-cli -g 
```

#### 2、项目生成依赖配置文件

```
npm init
```



### 二、基本认识

`webpack` 本质上是一个打包工具，它会根据代码的内容解析模块依赖，帮助我们把多个模块的代码打包。主要将不同类型的文件打包成几个几个必需的静态文件。`webpack` 有非常灵活的**配置项**和强大的**扩展能力**，所以使得前端打包变的十分的方便。

![webpack as a bundler](https://user-gold-cdn.xitu.io/2018/3/19/1623bfac4a1e0945?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### 1、入口

在多个代码块中都会有一个 `.js` 文件，其实这就是 webpack 打包的一个入口文件，打包的时候先读取这个文件，然后再开始解析这个文件所有的依赖。一般默认的入口文件路径为 `./src/index.js`。

对于单页应用来说，可能入口只有一个；如果多个页面的项目，就会对应多个构建入口。

> 单页应用和多页应用的区别？

```javascript
module.exports = {
  entry: './src/index.js' 
}

// 上述配置等同于
module.exports = {
  entry: {
    main: './src/index.js'
  }
}

// 或者配置多个入口
module.exports = {
  entry: {
    foo: './src/page-foo.js',
    bar: './src/page-bar.js', 
    // ...
  }
}

// 使用数组来对多个文件进行打包
module.exports = {
  entry: {
    main: [
      './src/foo.js',
      './src/bar.js'
    ]
  }
}
```



#### 2、loader

`loader` 是一种处理多种文件格式的机制。可以理解为将不同文件格式的内容转化为 weboack 可以打包的模块（也就是 js 模块）。通过不同的` loader` 来处理不同的文件格式，比如需要 `css-loader` 、`style-loader` 来处理 `.css` 文件，最终都将转化为 js 代码，因为 JS 代码可以在浏览器中运行。



我们主要在 `module.rules` 进行相关配置。

```javascript
module: {
  // ...
  rules: [
    {
      test: /\.jsx?/, // 匹配文件路径的正则表达式，通常我们都是匹配文件类型后缀
      include: [
        path.resolve(__dirname, 'src') // 指定哪些路径下的文件需要经过 loader 处理
      ],
      use: 'babel-loader', // 指定使用的 loader
    },
  ],
}
```

> PS:loader 在 webpack 中相对于比较重要。



#### 3、plugin

模块的代码转换需要 `loader` 来完成，而其他的任务，比如 `js` 的压缩，是通过 `uglifyjs-webpack-plugin` 插件来完成的。

```javascript
const UglifyPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  plugins: [
    new UglifyPlugin()
  ],
}
```

> PS: 开发者可以根据需求自己开发 plugins 插件。



#### 4、输出

输出则是指的是 webpack 打包之后输出的静态文件，我们可以配置**输出的路径和文件名**，最终通过 `out `字段来配置。

```js
module.exports = {
  // ...
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
}

// 或者多个入口生成不同文件
module.exports = {
  entry: {
    foo: './src/foo.js',
    bar: './src/bar.js',
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
  },
}

// 路径中使用 hash，每次构建时会有一个不同 hash 值，避免发布新版本时线上使用浏览器缓存
module.exports = {
  // ...
  output: {
    filename: '[name].js',
    path: __dirname + '/dist/[hash]',
  },
}
```



### 三、相关配置

> 一般我们会在项目下的 `webpack.config.js` 相关的 webpack 运行配置。

因为 `webpack.config.js` 是一个 `Node.js` 脚本，扩展性非常高，因为可以引入第三方模块使用。该配置文件通过暴露出一个配置对象，`webpack `在运行的时候会读取该对象的相关配置文件。

```javascript
const path = require('path')
const UglifyPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  entry: './src/index.js',

  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },

  module: {
    rules: [
      {
        test: /\.jsx?/,
        include: [
          path.resolve(__dirname, 'src')
        ],
        use: 'babel-loader',
      },
    ],
  },

  // 代码模块路径解析的配置
  resolve: {
    modules: [
      "node_modules",
      path.resolve(__dirname, 'src')
    ],

    extensions: [".wasm", ".mjs", ".js", ".json", ".jsx"],
  },

  plugins: [
    new UglifyPlugin(), 
    // 使用 uglifyjs-webpack-plugin 来压缩 JS 代码

  ],
}
```

处理之外，我们很少从零开始配置 `webpack`，而是通过已有的脚手架提供。



### 四、搭建基本的前端开发环境



































































