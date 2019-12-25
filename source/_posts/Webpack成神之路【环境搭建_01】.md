---
title: Webpack成神之路【环境搭建_01】
categories:
- 前端工具
- webpack
tags:
- 前端工具
- webpack
---



一文教你搭建 Webpack 在 VS code 中的环境搭建！

<!--more-->



### 一、为什么需要webpack?

> JS 的复杂性和需要很多依赖包，CSS,Less...新增样式的扩展写法的编译工作，所以我们不得不借助Webpack工具来做辅助。



### 二、什么是Webpack?

> `WebPack` 可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Sass，TypeScript等），并将其**转换**和**打包**为合适的格式供浏览器使用。3.0 版本出现了**优化**的作用。

- **打包： **可以把多个 `Javascript` 文件打包成一个文件，减少服务器压力和下载带宽。(模块 —> 静态优化)
- **转换： **把**拓展语言**转换成为普通的 `JavaScript`，让浏览器顺利运行。
- **优化：**前端变的越来越复杂后，性能也会遇到问题，而 `WebPack` 也开始肩负起了优化和**提升性能**的责任。



### 三、安装 Webpack

#### 1、前提

> 安装 Node.js ，用 npm 来进行安装，进入项目的根目录。



#### 2、初始化项目

```
//初始化项目
npm init 
```



#### 3、安装 webpack,并一路回车

> 主要目的就是生成 `package.json` 文件(当前项目的依赖模块，自定义的脚本任务等 ) ,[自行扩展到 Node相关知识。]()



##### 1）初始化目录

```npm
//初始化目录
$ npm init
```

> npm终端会问你关于项目的名称，描述等，如果你不考虑发布到npm上，这些内容都不重要， 直接回车初始化，根目录会生成 `package.json` 文件。

- **script 字段：** `scripts`指定了运行脚本命令的 `npm` 命令行缩写，比如start指定了运行 `npm run start`时，所要执行的命令。 
- **dependencies 字段：**`dependencies `字段指定了项目发布后运行所依赖的模块。 
- **devDependencies 字段：**`devDependencies `指定项目开发所需要的模块。



##### 2）安装webpack

> ① `--save` 将其 `webpack` 保存到 `package.json` 中。
>
> ② `-dev` 是只在开发环境下使用。

```
$ npm install webpack（@版本号） -g
```



##### 3）检测版本

> 如果报错，请检查版本是否错误，或者全局已经安装，本地安装就会报错（需要删除全局安装，然后进行本地安装）。

```
// 查看版本号
webpack -v 
```

```
// 全局卸载 webpack
npm  uninstall  webpack -g
```



### 四、问题

> 局部安装的 webpack  失效问题。

```
npm install webpack（@版本号） --save-dev
```

