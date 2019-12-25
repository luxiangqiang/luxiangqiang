---
title:  玩转 ES6 之【set和WeakSet数据结构】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

ES6中的数据结构，很有用处的新知识！

<!--more-->



### 1、set的声明

> Set和Array 的区别是 Set 不允许内部有重复的值，如果有只显示一个，相当于去重。 

```javascript
//Set
let setArr = new Set(['小鹿','xiaolu','三本'])
console.log(setArr)
```



### 2、Set值的增删查

###### ▉ add 增加

```javascript
let setArr = new Set(['小鹿','xiaolu','三本'])
console.log(setArr)
setArr.add('哈哈')
console.log(setArr)
```



###### ▉ delete 删除

```javascript
let setArr = new Set(['小鹿','xiaolu','三本'])
console.log(setArr)

setArr.add('哈哈')
console.log(setArr)

setArr.delete('小鹿')
console.log(setArr)
```



###### ▉ has 查找

```javascript
let setArr = new Set(['小鹿','xiaolu','三本'])
console.log(setArr.has('xiaolu'))
```



###### ▉ clear 清空

```javascript
setArr.clear();
console.log(setArr)
```



###### ▉ set的循环 for…of…循环： 

```javascript
let setArr = new Set(['小鹿','xiaolu','三本'])
for(let item of setArr){
    console.log(item)
}
```



###### ▉ size属性 

> 返回 set 中的数量。

```javascript
console.log(setArr.size)
```



###### ▉ forEach循环 

```javascript
setArr.forEach(
    (value)=>{
        console.log(value)
    }
)
```



### 3、WeakSet的声明 

> **注意：**这里需要注意的是，如果你直接在new 的时候就放入值，将报错， 而且不能传入重复的值。

```javascript
let weakObj = new WeakSet();
let obj = {a:'xiaolu',b:'小鹿'}
weakObj.add(obj);

console.log(weakObj)
```











