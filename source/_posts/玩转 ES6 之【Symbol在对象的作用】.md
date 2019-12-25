---
title:  玩转 ES6 之【Symbol在对象中的作用】
categories:
- 前端
- ES6
tags:
- 前端
- ES
---

Symbol，它的意思是全局标记。 

<!--more-->



#### 1、**声明Symbol** 

```javascript
var a = new String;
var b = new Number;
var c = new Boolean;
var d = new Array;
var e = new Object; 
var f= Symbol();
console.log(typeof(d));
```



#### 2、**Symbol的打印** 

```javascript
var g = Symbol('xiaolu');
console.log(g);
console.log(g.toString());
```



#### 3、**Symbol在对象中的应用** 

```javascript
var xiaolu = Symbol();
var obj={
    [jspang]:'小鹿'
}
console.log(obj[jspang]);
obj[jspang]='三本';
console.log(obj[jspang]);
```



#### 4、**Symbol对象元素的保护作用** 

> 循环的是时候不输出。

```javascript
let obj={name:'xiaolu',skill:'三本'};
let age=Symbol();
obj[age]=18;
for (let item in obj){
    console.log(obj[item]);
} 
console.log(obj);
```

