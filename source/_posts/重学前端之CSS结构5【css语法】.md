---
title: 重学前端之CSS结构5【css语法】
categories:
- 前端
- js
tags:
- 前端
- js
---

 CSS 语法规则，你需要知道的 CSS 知识！

<!--more-->

## 目录

- at 规则
  - 13 个例子
- 普通规则
  - 选择器
  - 声明区块
    - 属性
    - 值（函数）
  - 选择器语法结构



## CSS语法

> 1、任何的 CSS 的特性都必须通过一定的语法结构表达出来的，所以必须通过语法才会发现大多数 CSS 特性。
>
> 2、CSS 的顶层样式表是由两种规则组成的规则列表构成，一种是 at 规则，另一种是普通规则（qualified rule）。其中普通规则是开发中最常用到的。



### 一、at 规则

- **@ charset :** https://www.w3.org/TR/css-syntax-3/

> charset 用于提示 CSS 文件使用的字符编码方式。

```css
@charset "utf-8";
```



- **@ import :** https://www.w3.org/TR/css-cascade-4/

> @ import 用于引入css文件，引入另一个 javascript 文件的全部内容。

```css
@import "mystyle.css";
@import url("mystyle.css");
```



- **@ media :** https://www.w3.org/TR/css3-conditional/

> 它能够对设备的类型进行一些判断。

```css
@media print {
    body { font-size: 10pt }
}
```



- **@ page :** https://www.w3.org/TR/css-page-3/

> page 用于分页媒体访问网页时的表现设置，页面是一种特殊的盒模型结构，除了页面本身，还可以设置它周围的盒。

```css
@page {
  size: 8.5in 11in;
  margin: 10%;

  @top-left {
    content: "Hamlet";
  }
  @top-right {
    content: "Page " counter(page);
  }
}
```



- **@ counter-style :** https://www.w3.org/TR/css-counter-styles-3/

> counter-style 产生一种数据，用于定义列表项的表现。

```css
@counter-style triangle {
  system: cyclic;
  symbols: ‣;
  suffix: " ";
}
```



- **@ key-frames :** https://www.w3.org/TR/css-animations-1/

> keyframes 产生一种数据，用于定义动画关键帧。

```css
@keyframes diagonal-slide {

  from {
    left: 0;
    top: 0;
  }

  to {
    left: 100px;
    top: 100px;
  }

}
```



- **@ fontface :** https://www.w3.org/TR/css-fonts-3/

> fontface 用于定义一种字体，icon font 技术就是利用这个特性来实现的。

```css
@font-face {
  font-family: Gentium;
  src: url(http://example.com/fonts/Gentium.woff);
}

p { font-family: Gentium, serif; }

```



- **@ supports :** https://www.w3.org/TR/css3-conditional/

> support 检查环境的特性，它与 media 比较类似。



- **@ namespace :** https://www.w3.org/TR/css-namespaces-3/

> 用于跟 XML 命名空间配合的一个规则，表示内部的 CSS 选择器全都带上特定命名空间。



### 二、普通规则

> 普通规则：选择器 + 声明区块（属性 + 值 ）



#### 1、选择器：语法角度

**语法结构：**

- 普通规则
  - 选择器
  - 声明区块
    - 属性
    - 值
      - 值的类型
      - 函数

**语法结构：**

- complex-selector（单个元素选择器）
  - combinator   （连接符）
    - 空格
- \>
    - +
- ~
    - ||

  - compound-selector    （可复合选择器）
  - type-selector         （标签选择器）
    - subclass-selector
    - id (#)                （ID 选择器）
      - class		        （类选择器）
    - attribute         （属性选择器）
      - pseudo-class （伪类选择器）
  - pseudo-element  （伪元素选择器）



#### 2、声明：属性和值

> **属性**是由中划线、下划线、字母等组成的标识符，CSS 还支持使.用反斜杠转义。属性不允许使用连续的两个中划线开头，这样的属性会被认为是 CSS 变量。

```java
:root {
  --main-color: #06c;
  --accent-color: #006;
}
/* The rest of the CSS file */
#foo h1 {
  color: var(--main-color);
}
```



**CSS 属性值类型：**

- CSS 范围的关键字：initial，unset，inherit。
- 字符串：比如 content 属性。
- URL：使用 url() 函数的 URL 值。
- 整数 / 实数：比如 flex 属性。
- 维度：单位的整数 / 实数，比如 width 属性。
- 百分比：大部分维度都支持。
- 颜色：比如 background-color 属性。
- 图片：比如 background-image 属性。
- 2D 位置：比如 background-position 属性。
- 函数：来自函数的值，比如 transform 属性。



**CSS 支持的计算型函数：**

- calc ()

> calc()函数是基本的表达式计算，它支持加减乘除四则运算。

```css
section {
  float: left;
  margin: 1em; border: solid 1px;
  width: calc(100%/3 - 2*1em - 2*1px);
}
```



- max ()

- min ()
- clamp ()

> max()、min() 和 clamp()则是一些比较大小的函数。clamp() 则是给一个值限定一个范围，超出范围外则使用范围的最大或者最小值。



- toggle ()

> toggle() 函数在规则选中多于一个元素时生效,它会在几个值之间来回切换。

```css
ul { list-style-type: toggle(circle, square); }
```



- attr ()

> 函数允许 CSS 接受属性值的控制。
>



















