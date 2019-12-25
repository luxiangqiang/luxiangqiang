---
title: 前端 CSS3 之基础篇【选择器】
categories:
- 前端
- CSS3
tags:
- 前端
- CSS3
---

带你走进 CSS3 的边框与阴影！

<!--more-->



### 一、CSS3 圆角

#### 1、border - radius 属性

> 最多可以指定四个 border -*- radius 属性的符合属性，这个属性允许为你元素添加圆角边框！



######  ▉ 语法

```css
border-radius:1-4 length|%
```



######  ▉ 兼容性

> IE9+、FireFox4+、Chrome、Safari5+、Opera 

```css
//不同浏览器使用不同内核来渲染
-webkit-border-radius：50%;//谷歌浏览器
	-moz-border-radius:50%;//火狐浏览器
	 -ms-border-radius:50%;//IE 浏览器
	  -o-border-radius:50%;//Opera浏览器
		 border-radius:50%;
```

```css
<style>
    div{
        width: 800px;
        height: 300px;
        border: 5px solid red;
        margin: 0 auto;
        /* 左上角、右上角、右下角、左下角 */
        border-radius: 10px 20px 30px 40px;
    }
</style>
```



######  ▉ 指定每个圆角

- border-top-left-radius 		定义左上角的弧度
- border-top-right-radius 	        定义右上角的弧度
-  border-bottom-left-radius         定义左下角的弧度
-  border-bottom-right-radius 	 定义右下角的弧度



### 二、CSS 盒阴影属性

> 可以设置一个或多个下拉阴影的框。



#### 1、box-shadow 属性

###### ▉ 语法

```css
//水平偏移量，垂直偏移量，模糊，扩展(向周边长度进行扩展)，颜色，内阴影
box-shadow:h-shadow v-shadow blur spread color insert;
```



###### ▉ 兼容性

> IE9+、FireFox4+、Chrome、Safari5+、Opera



### 三、CSS 边界图片

#### 1、border-image 属性

> 使用 border-image- * 属性来构建魅力的可扩展按钮。



###### ▉ 语法

```css
// 
border-image:source slice width outset repeat;
```



###### ▉ 兼容性

> IE 不兼容、FireFox、Chrome、Safair6+、Opera 不兼容。

















