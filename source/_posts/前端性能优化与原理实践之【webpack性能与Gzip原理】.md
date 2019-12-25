---
title: 前端性能优化与原理实践之【webpack性能与Gzip原理】
categories:
- 前端
- 性能优化
tags:
- 前端
- 性能优化
---

【网络篇】前端性能优化之 webpack 与 Gzip 原理！

<!--more-->



### 一、前端的性能优化思路

从输入 URL 到页面加载完成，这个过程进行不断的优化、反复的琢磨， 把优化做到极致：

- DNS 解析；
- TCP 连接；
- HTTP 请求抛出；
- 服务端处理请求，HTTP 响应返回；
- 浏览器拿到响应数据，解析响应内容，把解析的结果展示给用户。

![img](https://user-gold-cdn.xitu.io/2018/10/23/1669f5358f63c0f8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 二、网络篇：webpack性能与Gzip原理

这输入 URL 到显示出页面这个过程中，网络部分有一下三个：

- DNS 解析
- TCP 连接
- **HTTP 请求/响应（网络优化的核心）**



#### 2.1 HTTP 优化

我们主要在 HTTP 请求有两个方面可以优化:

- 减少资源的请求次数
- 减少单次请求的时间

> 要想减少资源的请求次数，就把资源进行**合并**；要想减少单次请求的时间，就要**压缩**请求的资源。这些都要交给 webpack 去处理。



#### 2.2 webpack 的性能瓶颈

但是 webpack 也存在性能瓶颈，所以我们要对 webpack 进行优化，优化方向主要有两个：

- **webpack 的构建过程消费时间长**
- **webpack 打包后的体积还是太大**



#### 2.3 webpack 的优化

> 这一篇基本都是对 webpack 工具的使用优化，因为工具不断的进行迭代更新，所以并不作为重点进行优化，具体到实际项目中知道什么情况下要使用什么方法。工具永远在迭代，唯有掌握核心思想，才可以真正做到举一反三



#### 1、提速构建过程

- **不让 loader 做太多事情**
- **不让第三方库参与构建**
- **将 loader 有单进程转化为多进程**



#### **1.1 不让 loader 做太多事情：**

loader 很强大，同时又很慢，所以优化的最佳方式就是使用 include 或 exclude 来让 loader 转义不必要的包，比如依赖包 node_modules。这样，帮我们规避了对庞大的 node_modules 文件夹或者 bower_components 文件夹的处理。—— 但是优化是有限的。

```javascript
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```



开启文件缓存，将转译的文件结果缓存到文件系统中，这个过程**性能提升了 2 倍**。

```javascript
loader: 'babel-loader?cacheDirectory=true'
```



#### **1.2 不让第三方库参与构建：**

除了 `loader` 之外，还有 `Plugin` 插件库，也是非常庞大的，比如：`node_modules`。

经常处理第三方库的方法：

- **Externals：**一些情况下会引发重复打包的问题；
- **CommonsChunkPlugin：**每次构建时都会重新构建一次 vendor;

##### 最佳方案：处于效率考虑，使用 DllPlugin：

> DllPlugin 是基于 Windows 动态链接库（dll）的思想被创作出来的。这个插件会把第三方库单独打包到一个文件中，这个文件就是一个单纯的依赖库。**这个依赖库不会跟着你的业务代码一起被重新打包，只有当依赖自身发生版本变化时才会重新打包**。

使用方法：

- 基于 dll 专属的配置文件，打包 dll 库；
- 基于 webpack.config.js 文件，打包业务代码。

```javascript
const path = require('path')
const webpack = require('webpack')

module.exports = {
    entry: {
      // 依赖的库数组
      vendor: [
        'prop-types',
        'babel-polyfill',
        'react',
        'react-dom',
        'react-router-dom',
      ]
    },
    output: {
      path: path.join(__dirname, 'dist'),
      filename: '[name].js',
      library: '[name]_[hash]',
    },
    plugins: [
      new webpack.DllPlugin({
        // DllPlugin的name属性需要和libary保持一致
        name: '[name]_[hash]',
        path: path.join(__dirname, 'dist', '[name]-manifest.json'),
        // context需要和webpack.config.js保持一致
        context: __dirname,
      }),
    ],
}
```

得到结果：

```
vendor-manifest.json // 依赖文件
vendor.js // 第三方库
```

` webpack.config.js` 里针对 dll 稍作配置：

```javascript
const path = require('path');
const webpack = require('webpack')
module.exports = {
  mode: 'production',
  // 编译入口
  entry: {
    main: './src/index.js'
  },
  // 目标文件
  output: {
    path: path.join(__dirname, 'dist/'),
    filename: '[name].js'
  },
  // dll相关配置
  plugins: [
    new webpack.DllReferencePlugin({
      context: __dirname,
      // manifest就是我们第一步中打包出来的json文件
      manifest: require('./dist/vendor-manifest.json'),
    })
  ]
}
```



#### **1.3 Happypack——将 loader 由单进程转为多进程** 

> webpack 是单线程的，再多的任务只能排序处理。但是我们的 CPU 是多核的，Happypack 利用 CPU 多核的优势，把任务分发给子线程并发去执行，提高了打包的效率。



```javascript
const HappyPack = require('happypack')
// 手动创建进程池
const happyThreadPool =  HappyPack.ThreadPool({ size: os.cpus().length })

module.exports = {
  module: {
    rules: [
      ...
      {
        test: /\.js$/,
        // 问号后面的查询参数指定了处理这类文件的 HappyPack 实例的名字
        loader: 'happypack/loader?id=happyBabel',
        ...
      },
    ],
  },
  plugins: [
    ...
    new HappyPack({
      // 这个HappyPack的“名字”就叫做happyBabel，和楼上的查询参数遥相呼应
      id: 'happyBabel',
      // 指定进程池
      threadPool: happyThreadPool,
      loaders: ['babel-loader?cacheDirectory']
    })
  ],
}
```



#### 2、构建结果体积压缩

> 通过可视化打包工具查看各个模块打包后的大小，[webpack-bundle-analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)。使用方式如下：

```javascript
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
 
module.exports = {
  plugins: [
    new BundleAnalyzerPlugin()
  ]
}
```



#### **2.1拆分资源**

>  DllPlugin。
>



#### **2.2 删除冗余的代码**

> 使用 Tree-Shaking 来检测没有使用的模块，然后打包时自动会去除。
>

Tree-Shaking 只针对于模块级别的，如下边，只引入，并没有使用，所以在编译的时候被感知，打包的时候会被直接删除掉。

```
export const page1 = xxx

export const page2 = xxx
```

如果针对于更细节的代码冗余，需要在整合到 CSS 和 JS 时候进行分析。下面是在压缩时候，对冗余代码（注释、console等）的自动化删除。看一下 UglifyJsPlugin 插件压缩的时候(webpack3)。

```javascript
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
module.exports = {
 plugins: [
   new UglifyJsPlugin({
     // 允许并发
     parallel: true,
     // 开启缓存
     cache: true,
     compress: {
       // 删除所有的console语句    
       drop_console: true,
       // 把使用多次的静态值自动定义为变量
       reduce_vars: true,
     },
     output: {
       // 不保留注释
       comment: false,
       // 使输出的代码尽可能紧凑
       beautify: false
     }
   })
 ]
}
```

webpack4 使用  `uglifyjs-webpack-plugin` 对代码进行压缩。



#### **2.3 按需加载**

假如 React 使用单页应用，就会用 React-Router 来控制，一共十个页面，而且每个页面非常复杂，如果打包同时加载的时候，就会出现页面卡死状态，所以我们采用的方案就是用户需要显示哪一个就加载哪一个。

- **一次不加载完所有的文件内容，只加载此刻需要用到的那部分（会提前做拆分）**
- **当需要更多内容时，再对用到的内容进行即时加载**



**正常的路由组件：**

```javascript
import BugComponent from '../pages/BugComponent'
...
<Route path="/bug" component={BugComponent}>
```



**webpack 的配置文件：**

```javascript
output: {
    path: path.join(__dirname, '/../dist'),
    filename: 'app.js',
    publicPath: defaultSettings.publicPath,
    // 指定 chunkFilename
    chunkFilename: '[name].[chunkhash:5].chunk.js',
},
```



**路由处理：**

这是一个异步的方法，`webpack` 在打包时，`BugComponent` 会被单独打成一个文件，只有在我们跳转 bug 这个路由的时候，这个异步方法的回调才会生效，才会真正地去获取 `BugComponent` 的内容。这就是按需加载。

```react
onst getComponent => (location, cb) {
  require.ensure([], (require) => {
    cb(null, require('../pages/BugComponent').default)
  }, 'bug')
},
...
<Route path="/bug" getComponent={getComponent}>
```

> PS: 按需加载可以继续细化到每个更小的组件，或者某个功能点。



没错，在 React-Router4 中，我们确实是用 Code-Splitting 替换掉了楼上这个操作。而且如果有使用过 React-Router4 实现过路由级别的按需加载的同学，可能会对 React-Router4 里用到的一个叫“Bundle-Loader”的东西印象深刻。我想很多同学读到按需加载这里，心里的预期或许都是时下大热的 Code-Splitting，而非我呈现出来的这段看似“陈旧”的代码。

但是，如果大家稍微留个心眼，去看一下 Bundle Loader 并不长的源代码的话，你会发现它竟然还是使用 require.ensure 来实现的——这也是我要把 require.ensure 单独拎出来的重要原因。所谓按需加载，根本上就是在正确的时机去触发相应的回调。理解了这个 require.ensure 的玩法，大家甚至可以结合业务自己去修改一个按需加载模块来用。



### 三、HTTP Gzip 压缩算法

> **HTTP 压缩就是以缩小体积为目的，对 HTTP 内容进行重新编码的过程**,一个简单又好用的 HTTP 压缩操作：开启 Gzip。

只需在 HTTP 的头部，加上如下属性；

```javascript
accept-encoding:gzip
```



#### 3.1 Gzip 的原理

在一个文本文件中找出一些重复出现的字符串、临时替换它们，从而使整个文件变小。根据这个原理，**文件中代码的重复率越高，那么压缩的效率就越高**，使用 Gzip 的收益也就越大。反之亦然。



#### 3.2 什么情况下使用 Gzip 

通常很小的文件，不值得用 `Gzip`，压缩解压的时间都超过的传输的时间。如果用到比较大的项目文件，使用 `Gzip` 压缩，完全可以忽略压缩解压的过程。



#### 3.3 Gzip 的效率

- 优点：压缩后**通常**能帮我们减少**响应 70% 左右的大小**。
- 缺点：`Gzip `并不保证针对每一个文件的压缩都会使其变小。



#### 3.4 权衡压缩负载

 **Gzip 主要用于服务器端的操作，从而节省了传输的开销。**但是服务器 CPU 承担的压力很大的时候，就会出现服务器崩溃或者压缩很慢，所以前后台要进行权衡，有必要的时候对后台的服务器进行分压，使用 webpack 中的 Gzip 进行压缩。























