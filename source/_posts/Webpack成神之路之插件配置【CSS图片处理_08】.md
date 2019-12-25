---
title: Webpack成神之路之插件配置【CSS 图片处理_08】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---



webpack 处理 CSS 中的图片！

<!--more--> 



## 01|CSS 中的图片处理



#### 1、图片准备

> 通常我们保存的图片早 `src` 中的 `images` 文件夹下。



#### 2、安装解析图片的 loader

> 1）当我们直接用 `webpack` 命令时，就会报错，报错的原因就是无法解析 `CSS` 中的图片。之前解析 `CSS` 文件使用的 `loader` ，所以我们要添加专门解析图片用的 `loader`。
>
> 2）`webpack` 打包将各个模块打包成一个文件，所以我们的样式文件 `url` 是相对于 `CSS` 文件的，当我们打包成一个文件，`url`  的路径是相对于 `html` 而言的，导致原来的 `css` 文件引入的路径就会导致找不到原来的图片位置。
>
> 3）图片过多，导致有很多的 http 请求，降低页面的性能。

```javascript
npm install --save-dev file-loader url-loader
```

- **file-loader：**这个模块主要解决上述 （2）的问题。`file-loader` 可以解析项目中的 `ur` l引入（不仅限于 `css` ），根据我们的配置，将图片拷贝到相应的路径，再根据我们的配置，修改打包后文件引用路径，使之指向正确的文件。
- **url-loader：**这个模块主要解决上述 （3）的问题。`url-loader` 会将引入的图片编码，生成 `dataURl`。相当于把图片数据翻译成一串字符。再把这串字符打包到文件中，最终只需要引入这个文件就能访问图片了。当然，如果图片较大，编码会消耗性能。因此 `url-loader` 提供了一个 `limit` 参数，小于 `limit` 字节的文件会被转为 `DataURl`，大于 `limit` 的还会使用 `file-loader` 进行 `copy `.。



#### 3、配置 loader

> - `test:/.(png|jpg|gif)/：`是匹配图片文件后缀名称。
> - `use：`是指定使用的 `loader` 和 `loader` 的配置参数。
> - `imit：`是把小于 `500000B` 的文件打成 `Base64` 的格式，写入JS。

```javascript
module:{
    rules:[
        {
            test: /\.(png|jpg|gif)/,
            use: [{
                loader: 'url-loader',
                options:{
                    options:{
                        limit: 500000,
                        outputPath:'images/', // 将图片放到规定的目录下
                    }
                }
            }]
        } 
    ]
},
```



#### 4、url-loader 和 file-loader 的关系

> `url-loader `封装了 `file-loader`，配置中的 `limit` ，`url-loader` 会调用  `file-loader `进行处理。



## 02|CSS分离与图片路径处理

> 1）把 `CSS` 从J `avasScript` 代码中分离出来 。
>
> 2）如何处理分离出来后 `CSS` 中的图片路径不对问题 。



#### 1、CSS 分离

> 



#### 2、安装插件（extract-text-webpack-plugin）

> 注意版本号，我 webpack3.0 使用的是 2.0.1 版本的插件。

```javascript
npm install --save-dev extract-text-webpack-plugin@2.0.1
```



#### 3、配置文件

> 1）引入插件。
>
> 2）new 出插件对象。
>
> 3）修改 `style-loader` 和 `css-loader`。 

```javascript
// 引入插件
const extractTextPlugin = require('extract-text-webpack-plugin'); 
// 此路径是分离的 CSS 文件路径
new extractTextPlugin("/css/index.css")
```

```javascript
{
    test:/\.css$/,
        use:extractTextPlugin.extract({
            fallback: "style-loader",
            use: "css-loader"
        })
},
```



#### 4、路径失效问题





## 03|处理 HTML 中的图片

> 通常我们会在 HTML 中引入图片，需要对 HTML 中的图片进行打包。



#### 1、安装 Loader

```javascript
npm install html-withimg-loader --save
```



#### 2、配置 Loader

```javascript
{
    test: /\.(htm|html)$/i,
     use:[ 'html-withimg-loader'] 
}
```





