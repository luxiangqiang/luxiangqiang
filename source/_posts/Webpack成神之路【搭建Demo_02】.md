---
title: Webpack 成神之路【搭建 Demo_02】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---



Webpack 实现项目打包！

<!--more-->

### 一、项目基本目录

> 在根目录下建立两个文件夹，分别是 `src` 文件夹和 `dist` 文件夹。

- **src：**用来存放我们编写的 `javascript` 代码，可以简单的理解为用 `JavaScript` 编写的模块。
- **dist ：**用来存放供浏览器读取的文件，这个是webpack打包成的文件。



### 二、添加文件

> 1）在 `dist` 中我们建立 `index.html` 的文件并引入 `bubdle.js` 文件（`bubdle.js `是打包后的文件，在这先进行引入）。
>
> 2）在 `src` 下建立源文件 js，也就是我们所说的入口文件 `entery.js` ，用来编写 `js` 代码。 



### 三、打包 webpack 4.0+

> 在终端使用命令进行打包，命令格式如下：

```
// webpack 3.0+
wepack {入口文件路径} {出口文件路径}
//webpack 4.0+
webpack [入口文件路径]  --output [出口文件路径] --mode development
```



### 四、打包完成

> 运行完上方命令之后，在 `dist` 打包后的文件夹中出现 `bundle.js `打包文件，我们就可以在浏览器中进行预览了。

