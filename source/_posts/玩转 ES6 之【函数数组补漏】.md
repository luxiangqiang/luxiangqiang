---
title:  玩转 ES6 之【函数和数组补漏】
categories:
- 前端
- ES6
tags:
- 前端
- ES
---

数组和函数的其他使用！

<!--more-->

#### 1、对象的函数解构

> 后台传来的JSON格式数据当作参数，传递到函数内部进行处理。ES6就为供了这样的解构赋值，不用一个个传递参数了。 

```javascript
// 数组 JSON 解构
let json = {
    a:'xiaolu',
    b:'小鹿'
}
//b设置默认值
function fun({a,b='xiaolu'}){
    console.log(a,b);
}
fun(json);
```



#### 2、数组的函数解构

```
//解构数组
let arr = ['xiaolu','小鹿','三本'];
function fun(a,b,c){
    console.log(a,b,c);
}
fun(...arr);
```



#### 3、in的用法 

##### ■ 对象判断

> 判断某一个属性值是否在对象中。

```javascript

```



##### ■ 数组判断

```javascript
//ES5存在的弊端
let arr=[,,,,,];
console.log(arr.length); //5

// ES6中的 in 来解决
let arr=[,,,,,];
//这里的0指的是数组下标位置是否为空。
console.log(0 in arr); //false

let arr1=['xiaolu','小鹿'];
console.log(0 in arr1);  // true
```



#### 4、数组的遍历方法

##### ■ forEach 

> forEach循环的特点是会自动省略为空的数组元素，相当于直接给筛空了。 

```javascript
//遍历数组
let arr = ['xiaolu','小鹿','三本'];
arr.forEach((element,index) => {
    console.log(element+index)
});
```



##### ■ filter 

```javascript
//filter遍历
let arr = ['xiaolu','小鹿','三本'];
arr.filter(
    element =>console.log(element)
)
```



##### ■ some 

```javascript
//some 遍历
let arr = ['xiaolu','小鹿','三本'];
arr.some(element =>console.log(element))
```



##### ■ map 遍历

```
//map循环起到代替作用
let arr = ['xiaolu','小鹿','三本'];
console.log(arr.map(element=>'xiaolu'))
```



#### 5、数组转换为字符串

##### ■ join() 方法

```javascript
//‘|’隔开
let arr = ['xiaolu','小鹿','三本'];
console.log(arr.join('|'))
```



##### ■ toString() 方法

```
//逗号隔开
let arr = ['xiaolu','小鹿','三本'];
console.log(arr.toString())
```





































