---
title:  玩转 ES6 之【数字操作】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

如果你对数字操作的不好，就很难写出令人惊奇的程序 。

<!--more-->

### 一、进制声明

#### 1、二进制声明

> 二进制的英文单词是Binary，二进制的开始是0（零），然后第二个位置是b ，然后跟上二进制的值就可以了

```javascript
let binary = 0b010101;//21
```



#### 2、八进制声明

> 八进制的英文单词是Octal，也是以0（零）开始的，然后第二个位置是O（欧），然后跟上八进制的值就可以了。 

```javascript
let b=0o666;//438
```



### 二、数字判断和转换

#### 1、数字验证 **Number.isFinite( xx )** 

> 只要是数字，不论是浮点型还是整形都会返回true，其他时候会返回false。 

```javascript
let a= 11/4;
console.log(Number.isFinite(a));//true
console.log(Number.isFinite('xiaolu'));//false
```



#### 2、**NaN验证** 

> NaN是特殊的非数字，可以使用Number.isNaN()来进行验证。下边的代码控制台返回了true。

```javascript
console.log(Number.isNaN(NaN));
```



#### 3、判断是否为整数Number.isInteger(xx)

```javascript
let a=123.1;
console.log(Number.isInteger(a)); //false
```



#### 4、整数转换Number.parseInt(xxx)和浮点型转换Number.parseFloat(xxx)

```javascript
let s = 12.34
console.log(Number.parseInt(s))
console.log(Number.parseFloat(s))
```



#### 5、**整数取值范围操作** 

> 整数的操作是有一个取值范围的，它的取值范围就是2的53次方。 

```javascript
let a = Math.pow(2,53)-1;
console.log(a); //9007199254740991
```



#### 6、**最大安全整数** 

> 计算时会经常超出这个值，所以我们要进行判断，ES6提供了一个常数，叫做最大安全整数 

```javascript
consolec .log(Number.MAX_SAFE_INTEGER);
```



#### 7、**最小安全整数** 

```javascript
console.log(Number.MIN_SAFE_INTEGER);
```



#### 8、**安全整数判断isSafeInteger( )** 

```javascript
let a= Math.pow(2,53)-1;
console.log(Number.isSafeInteger(a));//false
```



9、









