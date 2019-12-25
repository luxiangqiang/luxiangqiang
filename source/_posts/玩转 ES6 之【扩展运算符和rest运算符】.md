---
title:  玩转 ES6 之【扩展运算符和rest运算符】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

两者都是（...）三个点，更好的为我们解决参数和对象数组未知情况下的编程，代码更健壮 。

<!--more-->



### 一、扩展运算符

#### 1、含义

> 1、扩展运算符可以将数组转化为用逗号分隔的**参数序列**。
>
> 2、该运算符主要用于函数的调用。
>
> 3、注意：只有**函数调用**扩展运算符时才能够放入到圆括号中，否则报错。
>
> 4、扩展运算符代替 apply 方法。
>
> 5、将一个数组拼接到另一个数组后边。

```javascript
// 1
console.log([1,2,3])    // 【1,2,3】
console.log(...[1,2,3]) //  1 2 3
```

```javascript
// 2
function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 将数组变为参数序列
```

```javascript
// 4
// 求数组中的最大值
Math.max.apply(null, [14, 3, 77]) // ES5 写法
Math.max(...[14, 3, 77])          // ES6 写法
// 等同于
Math.max(14, 3, 77);
```

```javascript
// 5
// 将一个数组拼接到另一个数组后边
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);// ES6 的写法
arr1.push(...arr2);// ES6 的写法
```



#### 2、扩展运算符的应用

##### 2.1 复制数组

> 数组是一个 object 类型，复制数组的时候，只复制了数组的地址，会导致牵一发而动千军。

```javascript
// ES5 变通的复制数组
const a1 = [1,2]
const a2 = a1.concat(); // 返回原数组的克隆(浅克隆)

// ES6 的修改
const a1 = [1,2]
const a2 = [...a1]      // 浅克隆
```



##### 2.2 合并数组

> 这两种方法都是对原数组的引用，如果修改了原数组，同步反映到新数组。

```javascript
// ES5 合并数组
arr1.concat(arr2,arr3)
// ES6 合并数组(外边为[]，内层的数组序列化开)
[...arr1, ...arr2, ...arr3]
```



##### 2.3 字符串

> 将字符串转为真正的数组。

```javascript
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```



### 二、rest 运算符

> 最基本用法。

```javascript
//rest 运算符
const xiaolu = (fist,...arg) =>{
    console.log(arg.length)
}
//first 代表 1 参数
xiaolu(1,2,3,4,5,6,7)//打印 7 ，说明 age 里边有 7 个参数
```



###### ▉ for...of 打印 arg 的值

```javascript
const xiaolu = (fist,...arg) =>{
    for(let a of arg){
        console.log(a)
    }
}
xiaolu(1,2,3,4,5,6,7)//打印 2,3,4,5,6,7
```













