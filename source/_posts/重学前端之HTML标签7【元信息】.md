---
title: 重学前端之HTML标签7【元信息】
categories:
- 前端
- HTML
tags:
- 前端
- HTML
---

head 里一共能写哪几种标签？

<!--more-->



## 元信息类标签

> 所谓元信息，描述自身的信息，元信息类标签，就是 HTML 用于描述文档自身的一类标签。



### ■ head 标签

> html 标签的第一个标签，必须包含一个 title，并且最多只能包含一个 base ，如果文档作为 iframe，或者有其他方式指定了文档的标题，可以允许不包含 title 标签。



### ■ title 标签

> title 表示文档的标题。



#### 1、title 和 h1 的区别

> - title 作为元信息，可能会被用在浏览器收藏夹、微信推送卡片、微博等各种场景，这时侯往往是上下文缺失的，所以 title 应该是完整地概括整个网页内容的。
> - h1 仅仅表示页面展示，默认具有上下文，并具有链接辅助，所以可以简写。



### ■ base 标签

> 给页面上所有的 URL 相对地址提供一个基础。实际开发中建议使用 javascript 来代替 base 标签，因为容易跟 javascript 造成配合问题。



### ■ meta 标签

> 是一组键值对，是一种通用的元信息表示标签。一般的 meta 标签由 **name** 和 **content** 两个属性来定义。name 表示**元信息的名**，content 则用于表示**元信息的值**。

```html
  <meta name=application-name content="lsForums">
```



#### 1、具有 charset 属性的 meta

> HTML 5 开始简写法，添加的 meta 没有了 name 和 content 属性，放到 head 的第一个。

```html
<html>
<head>
<meta charset="UTF-8">
……
```

> 1、浏览器在读到此标签之前，所有处理的字符是 ASCll 字符(ASCll 字符是 UTF-8 字符编码的子集)。
>
> 2、http 服务端会通过 http 头来指定正确的编码方式，但是有些特殊的情况如使用 file 协议打开一个 HTML 文件，则没有 http 头，这种时候，charset meta 就非常重要了。



#### 2、具有 http-equiv 属性的 meta 

> 表示执行一个命令。

```html
<!-- 相当于添加了 content-type 这个 http 头，并且指定了 http 编码方式 -->
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
```



content - type 几种命令：

- content-language 指定内容的语言；
- default-style 指定默认样式表；
- refresh 刷新；
- set-cookie 模拟 http 头 set-cookie，设置 cookie；
- x-ua-compatible 模拟 http 头 x-ua-compatible，声明 ua 兼容性；
- content-security-policy 模拟 http 头 content-security-policy，声明内容安全策略。



#### 3、name 为 viewport 的 meta

> 是移动端开发的事实标准。

```html
<meta name="viewport" content="width=500, initial-scale=1">
```



- **width**：页面宽度，可以取值具体的数字，也可以是 device-width，表示跟设备宽度相等。
- **height**：页面高度，可以取值具体的数字，也可以是 device-height，表示跟设备高度相等。
- **initial**-**scale**：初始缩放比例。
- **minimum-scale**：最小缩放比例。
- **maximum-scale**：最大缩放比例。
- **user-scalable：**是否允许用户缩放。



```html
<!--如果已经做好移动端适配的网页，应该把用户的缩放功能禁止掉，宽度为设备宽度-->
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no">
```















