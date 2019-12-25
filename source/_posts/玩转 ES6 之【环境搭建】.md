---
title:  玩转 ES6 之【环境搭建】
categories:
- 前端
- ES6
tags:
- 前端
- ES
---

VSCode ES6  Babel  环境搭建。

<!--more-->



### 一、ES6 环境搭建

> 目前 Chrome 浏览器已经支持 ES6 了，但是很多低版本的浏览器不支持 ES6 语法，所以需要将 ES6 转换成 ES5 来进行打包执行，除了 Webpack 支持自动编译转换的能力，Babel 也可以完成，下边用 Babel 来搭建 ES6 环境。



#### 1、初始化项目

> 安装 Babel 之前，需要初始化项目，打开终端或者 cmd 进入项目目录下，输入以下命令，项目根目录会出现 package.json 配置文件，可进行修改。

```javascript
// -y 默认全部同意
npm init -y
```



#### 2、全局安装 Babel-cli

> 在终端输入安装命令，npm 或者 cnpm 进行安装。 

```
// -g 全局
npm install -g babel-cli
```

注意：这时输入命令`babel [用ES6写的js目录/index.js] -o [转换成ES5的目录/index.js]`虽然可以在新转换成ES5的目录中出现新转换的文件夹，但是里边的内容还是ES6语法。



#### 3、安装 babel-preset-es2015 和 babel-cli 转换包

```
npm install --save-dev babel-preset-es2015 babel-cli
```

安装完成之后，`package.json` 文件多了 `devDependencies ` 选项。

```
"devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-es2015": "^6.24.1"
  }
```



##### ▉ 新建 .babelrc

在根目录下新建 `.babelrc` 文件，MAC 可能此后缀文件不支持，可上网自行搜索方法，让其支持此后缀。打开文件输入一下内容：

```
{
    "presets":[
        "es2015"
    ],
    "plugins":[]
}
```



##### ▉  转化命令

> 完成上边的安装之后，我们开始进行转换。下边这种命令转换非常繁琐，我们可以进一步进行简化。

```
babel src/index.js -o dist/index.js
```



**简化：**

```
{
  "name": "ReactDome",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    //修改这一行，冒号前边是命令，后边是转化路径
    "bulid": "babel [用ES6写的js目录/index.js] -o [转换成ES5的目录/index.js]" 
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-es2015": "^6.24.1"
  }
}

```



**输入一下命令将ES6 转化 ES5：**

```
npm run build
```





