---
title: Webpack成神之路之模块【css文件打包_05】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---



将 CSS 文件进行打包！

<!--more-->



## 模块：CSS 文件打包

> `Webpack` 在生产环境中有一个重要的作用就是减少 `http` 的请求数，就是把多个文件打包到一个 js 里，这样请求数就可以减少好多。 在对 CSS 打包之前，需要进行配置。



### 一、Loaders

> `Loaders` 是 `Webpack` 最重要的功能之一， `Loader` **对不同的文件进行处理**。

- 可以把 `SASS` 文件的写法转换成 `CSS`，而不在使用其他转换工具。
- 可以把 `ES6` 或者 `ES7` 的代码，转换成大多浏览器兼容的 `JS` 代码。
- 可以把 `React` 中的 `JSX` 转换成 `JavaScript` 代码。



#### 1、安装 Loader

> `Loader` 有两个解析的 `Loader` 分别为 `style-loader` 和 `css-loader`。
>
> 1）**style-loader：**用来处理 `CSS` 文件中的 `url()` 等（https://www.npmjs.com/package/style-loader ）。
>
>  **2）css-loader：**用来将 `CSS` 插入到页面中的 `style` 标签中(https://www.npmjs.com/package/css-loader )。

```javascript
//安装style-loader
npm install style-loader --save-dev

//安装css-loader
npm install --save-dev css-loader
```



#### 2、配置 Loader

> 在 webpack.config.js 进行配置。

- **test：**用于匹配处理文件的扩展名的表达式，这个选项是必须进行配置的；
- **use： **loader名称，就是你要使用模块的名称，这个选项也必须进行配置，否则报错；
- **include/exclude: **手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
- **query：为loaders**提供额外的设置选项（可选）。

```javascript
//直接用use
module:{
    rules: [
        {
       		//用于匹配处理文件的扩展名的表达式，这个选项是必须进行配置的；
            test: /\.css$/,
            //loader名称，就是你要使用模块的名称，这个选项也必须进行配置，否则报错；
            use: [ 'style-loader', 'css-loader' ]
        }
    ]
},
```

```javascript
//把 use 换成 loader。
module:{
        rules:[
            {
                test:/\.css$/,
                loader:['style-loader','css-loader']
            }
        ]
    },
```

```javascript
//用 use + loader 的写法：
module:{
    rules:[
        {
            test:/\.css$/,
            use: [
                {
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }
            ]
        }
    ]
},
```





#### 3、打包 CSS

> 在  `js` 文件中引入 `css` 文件。

```
import css from './src/index.css';
```

```
//命令进行打包
webpack
```





