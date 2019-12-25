---
title:  玩转 ES6 之【map】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

map是一种灵活，简单的适合一对一查找的数据结构。 

<!--more-->



### 1、Json和map格式的对比

> map的效率和灵活性更好 ,可以把它看成一种特殊的键值对，但你的key可以设置成数组，值也可以设置成字符串。

```javascript
let json = {
    name:'xiaolu',
    skill:'小鹿'
}
console.log(json.name);

let map = new Map();
map.set(json,'123')
console.log(map)
```



### 2、Map 的增删改

###### ▉ get 取值

```javascript
console.log(map.get(json))
```



###### ▉ delete 删除

```
map.delete(json)
console.log(map)
```



###### ▉ has 查找

```javascript
console.log(map.has(json))
```



###### ▉  清楚所有元素clear

```javascript
console.log(map.clear)
```



###### ▉ size 属性

```javascript
console.log(map.size)
```

























