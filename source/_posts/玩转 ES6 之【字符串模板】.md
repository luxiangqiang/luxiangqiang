---
title:  玩转 ES6 之【字符串模板】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

前后端分离经常用到拼接。

<!--more-->



### 字符串拼接

#### 1、简单拼接

> 使用 ${} 进行拼接。

```javascript
// 注意使用的 ` 符号
let lu = 'xiaolu';
let blog = `${lu} 你好<br>呀`; //在里边可以加标签
document.write(blog)
```



#### 2、对运算符的支持

```javascript
let a = 1;
let b = 2;
let c = `${a+b}`
document.write(c)
```



#### 3、字符串查找

###### ▉ 查找是否存在

```javascript
let xiaolu='小鹿';
let blog = '大家好，我是小鹿。';
document.write(blog.includes(xiaolu))//返回true
```



###### ▉ **判断开头是否存在** 

```
blog.startsWith(xiaolu);
```



###### ▉ **判断开头是否存在** 

```
blog.endsWith(xiaolu);
```



###### ▉ **复制字符串** 

```
document.write('xiaolu|'.repeat(3));
```

