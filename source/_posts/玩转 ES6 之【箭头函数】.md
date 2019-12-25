---
title:  玩转 ES6 之【箭头函数和扩展】
categories:
- 前端
- ES6
tags:
- 前端
- ES6

---

箭头函数的使用！

<!--more-->

### 一、箭头函数

> 注意箭头函数不能当做 构造函数来使用。



#### 1、箭头函数

```javascript
//普通函数
function add(a,b=1){
    return a+b;
}
//箭头函数
var add = (a,b=1)=>a+b;
console.log(add(1))
```



#### 2、{ } 使用

> 如果函数体内多余两条语句，就使用 {}。

```javascript
var add =(a,b=1) => {
    console.log('jspang')
    return a+b;
};
console.log(add(1));
```





