---
title:  玩转 ES6 之【ES6对象】
categories:
- 前端
- ES6
tags:
- 前端
- ES
---

对象对于Javascript是非常重要的。在ES6中对象有了很多新特性。 

<!--more-->

#### 1、对象赋值

> ES6允许把声明的变量直接赋值给对象。

```javascript
//ES6对象
let name="xiaolu";
let skill= '小鹿';
var obj= {name,skill};
console.log(obj);  
```



#### 2、对象 Key 值的构建

> 获取对象键值对 key 值，我们自己定义键值对 key 值。
>

```javascript
//对象构建key值
let key='skill';
var obj={
    [key]:'小鹿'
}
console.log(obj.skill);
```



#### 3、**自定义对象方法** 

```javascript
var obj={
    add:function(a,b){
        return a+b;
    }
}
console.log(obj.add(1,2));  //3
```



#### 4、**Object.is( ) 对象比较** 

> ES6为我们提供了is方法进行对比。

```javascript
var obj1 = {name:'xiaolu'};
var obj2 = {name:'xiaolu'};
console.log(obj1 === obj2);//false
console.log(Object.is(obj1.name,obj2.name)); //true
```

> 区分 === 和 is方法的区别是什么 

```
//===为同值相等，is()为严格相等。
console.log(+0 === -0);  //true
console.log(NaN === NaN ); //false
console.log(Object.is(+0,-0)); //false
console.log(Object.is(NaN,NaN)); //true
```



#### 5、Object.assign( )合并对象

```javascript
var a={a:'xiaolu'};
var b={b:'小鹿'};
var c={c:'三本'};
let d=Object.assign(a,b,c)
console.log(d);
```



