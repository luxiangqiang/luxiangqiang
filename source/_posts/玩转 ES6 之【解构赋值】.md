---
title: 玩转 ES6 之【解构赋值】
categories:
- 前端
- ES6
tags:
- 前端
- ES
---

解构赋值在实际开发中可以大量减少我们的代码量，并且让我们的程序结构更清晰，主要应用于**解析 JSON** 。

<!--more-->

### 变量的解构赋值

> **应用：**一般应用于解构 JSON 数据，非常方面快捷。



#### 1、数组的结构

###### ▉ 简单数组结构

```javascript
//一般形式
let a=0;
let b=1;
let c=2;

//解构形式
letl  [a,b,c]=[1,2,3];
```



###### ▉ 数组和赋值模式统一

> 等号左右要统一，否则可能获得 undefined。

```javascript
let [a,[b,c],d] = [1,[2,3],4];
```



###### ▉ 解构的默认值

> 需要注意 undefined 和 null 的区别。undefined 代表什么都没有，null代表有值。

```javascript
let [foo = true] = [];
console.log(foo); //控制台打印出true
```



#### 2、对象的解构

> **与数组解构的不同点：**数组的元素是按次序排列的 ，而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

```javascript
let {foo,bar} = {foo:'xiaolu',bar:'小鹿'};
```



###### ▉ 圆括号的使用

```javascript
//如果之前已经解构，现在就会报错
let foo;
{foo} ={foo:'xiaolu'};

//需要加圆括号
let foo;
({foo}={foo:'xiaolu'});
```



#### 3、字符串解构

> 字符串被转换成了一个类似数组的对象 。

```javascript
const [a,b,c,d,e,f]="JSPang";
```































