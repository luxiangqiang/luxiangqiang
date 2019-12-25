---
title: 前端 HTML5 基础篇之【属性变化】
categories:
- 前端
- HTML5
tags:
- 前端
- HTML5
---

第二节：HTML5 的属性添加了哪些？

<!--more-->



## HTML5 属性变化

### 目录

- Input 标签
- 表单属性
- 链接属性
- 其它属性



### 一、Input 属性

> **针对于 iphone 手机端(5个)：**email /url /tel /数字/Date Pickers
>
> **针对于客户端（3个）：**Range / Search /Color



#### 1、email 类型

> 针对于手机端，出现特殊的键盘，电子邮件 Input 类型。

```html
//用于手机输入邮件
<input type="email" name="email" >
```



#### 2、url 类型

> 之应用于苹果手机端，特殊键盘。

```html
<input type="url" name="url">
```



#### 3、tel 类型

> 出现特殊的电话号码键盘。

```html
  <input type="tel" name="tel">
```



#### 4、数字类型

> 出现数字运算键盘。

```html
<input type="number" name="number">
```



#### 5、Date Pickers类型

> 日期类型。**手机端只应用于 iphone**。

- **Date** —— 选取日、月、年
- **Month** —— 选取月年
- **Week ** —— 选取周和年
- **Time** —— 选取时间（小时和分钟）
- **Datetime** —— 选取时间、日、月、年（UTC时间）
- **Datetime-local** —— 选取时间、日、月、年（本地时间）

```html
 // 手机出现滚动日期
 date:<input type="date" name="date"><br>//年、月、日
 month:<input type="month" name="month"><br>//年、月
 week:<input type="week" name="week"><br>//年、周
 time:<input type="time" name="time"><br>//时、分
 datetime:<input type="datetime" name="datetime"><br>//年、月、日、时区
 datetime-local:<input type="datetime-local" name="datetime-local"><br>//年、月、日
```

> **Datetime 和 Datetime-local  的区别？**
>
> **1)兼容性。**Datetime 类型只有 Safire 和 Opera 浏览器兼容；而  Datetime-local 兼容 Chrome 、 Safire 和 Opera。
>
> **2) 返回类型不同。**local  返回本地时间，而 Date 返回时区。



#### 6、Range 类型

> 范围，默认范围0—100。

```html
<input type="range" name="range" min="1" max="10">
```



#### 7、search 类型

> 搜索框。

```html
<input type="search" name="search">
```



#### 8、Color 类型

> 出现颜色色板。

```html
<input type="color" name="color">
```



### 二、表单属性（5个）

#### 1、autocomplete 属性

> 当重新加载页面时，输入框重置，是否提示。autocomplete="on/off"

```html
<form action="" autocomplete="on">
    <input type="text" name="text">
    <input type="email" name="email" autocomplete="off">
    <input type="submit">
</form>
```



#### 2、aotufocus 属性

> 页面加载时，自动获取属性。

```html
<input type="text" name="text" autofocus="autofocus">
```



#### 3、multiple 属性

> 规定输入域可选择多个值。一般应用于**上传文件**（file）和**邮件 （email ）**输入框。



#### 4、placeholder 属性

> 提供一种提示（hint）。

```html
<input type="text" name="text" placeholder="请输入您的用户名">
```

**PS：**适用于以下几个 input 标签：（6个）

text，search，url，tel，email，password。



#### 5、require 属性

> 主要用来进行输入域验证（不能为空），必填字段。

```html
//填写完整才能进行提交
<input type="email" name="email" required="required">
```

**PS：适用于以下几个 input 标签：**

text，search，url，telephone，email，password，date，pickers，number，checkbox，radio，file。



### 三、链接属性（3个）

#### 1、sizes

> 根据屏幕不同的分辨率来调整不同的sizes。

```
<link rel="icon" href="icon.gif" type="image/gif" size="16X16">
```



#### 2、target

> base标签写在<hread>之间。

```html
//控制所有的页面的超链接默认选择新窗口。
<base href="http://localhost/" target="_blank">
```



#### 3、超链接

```
a：media=""//表示对设备进行优化，handhelp对手持设备进行支持，tv 对“电视”设备进行支持。
a:hreflang="zh"//设置语言，这里设置的中文
a:rel="external" //这里的超链接为外部链接
```



### 三、其他属性（4个）

#### 1、script 标签

> defer 属性：（只兼容 IE 浏览器）加载完浏览器之后，再加载 js 外部文件夹。
>
> async 属性：（兼容一切浏览器）加载页面的同时也加载外部文件。



#### 2、ol

> Start —— 起始值：有序列表的起始值。
>
> Reversed —— 有序列表倒序输出。



#### 3、html

> 定义页面的离线文件。

```
<html mainifest="index.mainifest"
```



#### 4、*style

> scoped：内嵌CSS。

```
<style scoped>
</style>
```



