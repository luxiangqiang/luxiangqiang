---
title: 前端 CSS3 之基础篇【选择器】
categories:
- 前端
- CSS3
tags:
- 前端
- CSS3
---

CSS3 所有基本选择器（必会）！

<!--more-->



## 基本选择器

### 一、回顾选择器（5个）

> 1、通配符选择器 `*{}`
>
> 2、元素选择器 `h1{}`
>
> 3、类选择器 `.container{}`
>
> 4、ID选择器 `#id{}`
>
> 5、后代选择器 `ul em{ }`



### 二、新增基本选择器（4个）

> 子元素选择器，相邻兄弟元素选择器，通用兄弟选择器，群组选择器。



#### 1、直接后代选择器（子元素选择器）

> 只能选择某元素的子元素。



###### ▉ 语法格式

> 父元素 > 子元素 （Father > Children）



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。



```html
<style>
    section>div{
    	color: #0f0f;
    }
</style>
<section>
	//这个div变色
    <div>外边的DIV</div>
    <article>
        <div>里边的DIV</div>
    </article>
</section>
```



#### 2、相邻兄弟选择器

> 相邻兄弟选择器可以选择紧接在另一元素后的元素，而且他们具有一个相同的父元素。



###### ▉ 语法格式

> 元素 + 兄弟相邻元素。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。



```html
<style>
    section>div + article{
        color: #0f0f;
    }
</style>
<section>
    <div>外边的DIV</div>
    <!-- 控制这一个 -->
    <article>
   	 	<div>里边的DIV</div>
    </article>
    <article>
    	<div>里边的DIV</div>
    </article>
</section>
```



#### 3、通用兄弟选择器

> 选择某元素后边的多有兄弟元素，而且他们具有一个相同的父元素。



###### ▉ 语法格式

> 元素 ~ 后面所有兄弟相邻元素



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。



```html
 <style>
     section>div ~ article{
     	color: #0f0f;
     }
 </style>
 <section>
    <!-- div 前边的 article 不会被选择 -->
    <div>外边的DIV</div>
    <!-- 控制这一个 -->
    <article>
   	 	<div>里边的DIV</div>
    </article>
     <!-- 也控制这一个 -->
    <article>
    	<div>里边的DIV</div>
    </article>
</section>
```



#### 4、群组选择器

> 相同元素的样式分组在一起，每个选择器之前使用逗号 “，” 隔开。



###### ▉ 语法格式

> 元素，元素，...，元素n 



###### ▉ 兼容性

> IE6+、FireFox、Chrome、Safari、Opera。

```css
section > div,
section > article,
section > aside{
	color:#000;
}
```



### 三、伪类选择器（5个）

#### 1、动态伪类选择器

> 并不存在 HTML 中，只有当用户和网站交互的时候才能体现出来。



###### ▉ 锚点伪类

> `: link`，`: visited`。



###### ▉ 用户行为伪类

> `: hover`（必须写在 link 和 visited 才会生效）, `: active`（必须写在 hover 后才会生效）,` : focus`(用于input输入)



#### 2、UI元素状态伪类

> 把“：enabled”，“：disabled”，“**：checked”** 伪类称为UI元素状态伪类。



###### ▉ 兼容性

> IE9+、FireFox、Chrome、Safari、**Opera**。

```html
<style>
    input:enable{
        width:100px;
        height:100px;
   	 	background: #0000ff;
    }
    //没有宽和高
    input:disabled{
   	 	background: #0000ff;
    }
</style>
<input type="text" disabled="disabled">
```



#### 3、否定选择器

> :not （Element/selector）选择器匹配非指定元素/选择器的每个元素。



###### ▉ 语法格式

> 父元素：not(子元素/子选择器)



###### ▉ 兼容性

> IE9+、FireFox、Chrome、Safari、Opera。

```css
nav > a:not(:last-of-type){
    border-right:1px solid red;
}
```



### 四、伪元素选择器

> 伪元素用于向某些选择区设置特殊效果。

- `:first-line`: 应用于多行文本的首行元素。应用于块元素。
- `:first-letter`:  选取首字母。应用于块元素。 
- `:first-child`:选择父元素的第一个子元素。注意：在 IE 中声明<!DOCTYPE> 才生效！
- `:before`:元素之前添加内容
- `:after` : 元素之后添加内容 



### 五、属性选择器

> `Element[attribute]`、`Element[attribute=  “value” ]`、`Element[attribute~=  “value” ]`、`Element[attribute^=  “value” ]`、`Element[attribute$=  “value” ]`、`Element[attribute*=  “value” ]`、`Element[attribute|=  “value” ]`。



#### 1、Element[attribute]

> 为带有 attribute 属性的 Element 元素设置样式。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。

```html
  <style>
      a[href]{
      	text-decoration: none;
      }
  </style>
  <a href="attribute.html">atribute</a>
```



#### 2、Element[attribute=  “value” ]

> 为 attribute= "value" 属性的 Element 元素设置样式。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。

```css
 a[href = "attribute.html"]{
     text-decoration: none;
     color: #f0f; 
 }
```



#### 3、Element[attribute~=  “value” ]

> 选择 attribute 属性包含单词 “value” 的元素，并设置其样式。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。

```html
<style>
    a[class ~= "two"]{
        text-decoration: none;
        color: #f0f; 
    }
</style>

<a class="one two" href="attribute.html1">atribute</a>
<a class="two three" href="attribute.html2">atribute</a>
```



#### 4、Element[attribute^=  “value” ]

> 选择以 value 开头的元素。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。

```html
 <style>
     a[href ^= "attribute.html"]{
         text-decoration: none;
         color: #f0f; 
     }
 </style>
  <a href="attribute.html">atribute</a>//变
  <a class="one two" href="attribute.html1">atribute</a>
  <a class="two three" href="attribute.html2">atribute</a>
  <a href="attribute.html3">atribute</a>//变
  <a href="attribute.html4">atribute</a>//变
  <a href="attribute.html5">atribute</a>//变
```



#### 5、Element[attribute$=  “value” ]

> 选择以 value 结尾的元素。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。



#### 6、Element[attribute*=  “value” ]

> 选择包含 value 的元素。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。



#### 7、Element[attribute|=  “value” ]

> 选择为 “value” 或者以 “value-” 开头的元素，并设置其样式。



###### ▉ 兼容性

> IE8+、FireFox、Chrome、Safari、Opera。



### 六、CSS 权重

#### 1、什么是权重？

> 很多规则被应用于某一个元素上，权重就是那中规则生效。或者说是优先级。



#### 2、权重等级与权值

> **行内样式**（1000）> **ID 选择器**（100）>**类、属性选择器和伪类选择器**（10）>**元素和伪类**（1）> ***** (0)。





























