---
title:  玩转 ES6 之【新增数组】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

ES6 中新增的数组知识！

<!--more-->

### 一、ES6中新增的数组知识

> 在ES6中绝大部分的Array操作都存在于Array对象里。



#### 1、**JSON数组格式转换** 

```javascript
// JSON 转换数组
let json = {
    '0':'xiaolu',
    '1':'小鹿',
    '2':'小鹿哈哈',
    length:3
}
let arr = Array.from(json)
console.log(arr)
```



#### 2、**Array.of()方法** 

> 负责把一堆文本或者变量转换成数组。 

```javascript
let arr = Array.of(1,2,3,4);
let arrText = Array.of('小鹿','小明','小强')
console.log(arr)
console.log(arrText)
```



#### 3、find()实例方法

> 所谓的实例方法就是并不是以Array对象开始的，而是必须有一个已经存在的数组，然后使用的方法，这就是实例方法。

- value：表示当前查找的值。
- index：表示当前查找的数组索引。
- arr：表示当前数组。

```javascript
//find() 实例方法
let arr=[1,2,3,4,5,6,7,8,9];
console.log(arr.find(function(value,index,arr){
    return value > 10;
}))
//控制台输出了6，说明找到了符合条件的值，并进行返回了，如果找不到会显示undefined。
```



#### 4、fill() 实例方法

> 用于把数组进行填充 。

- 参数一：填充的变量 
- 参数二：开始填充的位置 
- 参数三：填充到的位置 

```javascript
//fill()填充方法
let arr=[0,1,2,3,4,5,6,7,8,9];
arr.fill('小鹿',2,5);
//把数组从第二位到第五位用xiaolu进行填充
console.log(arr);//[0, 1, "小鹿", "小鹿", "小鹿", 5, 6, 7, 8, 9]
```



#### 5、数组的遍历

> for…of循环。

```javascript
//for-of循环
let arr = ['小鹿','xiaolu','啦啦']
for(let item of arr){
    console.log(item)
}
```

> 输出数组索引。

```javascript
for(let index of arr.keys()){
    console.log(index)
}
```

> 同时输出索引和元素。

```javascript
for(let [index,val] of arr.entries()){
    console.log(index+':'+val);
}
```



#### 6、**entries( )实例方法** 

> entries()实例方式生成的是Iterator形式的数组，需要时用next()手动跳转到下一个值。 

```javascript
let arr = ['小鹿','xiaolu','啦啦']
let list = arr.entries();
console.log(list.next().value);
console.log(list.next().value);
console.log(list.next().value);
//[0, "小鹿"]
//[1, "xiaolu"]
//[2, "啦啦"]
```





















