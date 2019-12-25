---
title:  玩转 ES6 之【promise对象】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

ES6为我们提供了类的使用。需要注意的是在写类的时候和 ES5 中的对象和构造函数要区分开来，不要学混了。 

<!--more-->



### 1、类的声明

> 最简单的类只有一个方法。

```javascript
class coder{
    name(val){
        console.log(val);
    }
}
```



### 2、类的使用

```javascript
//类的使用
class Coder{
    name(val){
        console.log(val)
    }
}

let xiaolu = new Coder;
xiaolu.name('小鹿')
```



### 3、类的多方法声明 

> 这里需要注意的是两个方法中间不要写逗号了，还有这里的this指类本身，还有要注意return 的用法。 

```javascript
//类的使用
class Coder{
    name(val){
        console.log(val)
        return val
    }
    skill(val){
        console.log(this.name('xiaolu')+':'+'Skill:'+val)
    }
}

let xiaolu = new Coder;
xiaolu.name('小鹿')
xiaolu.skill('三本')
```



### 4、**类的传参** 

> 在类的参数传递中用 constructor() 进行传参。传递参数后可以直接使用 this.xxx 进行调用。 

```javascript
//类的使用
class Coder{
    name(val){
        console.log(val)
        return val
    }
    skill(val){
        console.log(this.name('xiaolu')+':'+'Skill:'+val)
    }
    constructor(a,b){
        this.a = a;
        this.b = b;
    }

    add(){
        return this.a+this.b;
    }
}

let xiaolu = new Coder(1,2);
console.log(xiaolu.add())
```



### 5、class的继承

```javascript
class Html extends Coder{

}

let l  = new Html
l.name('哈哈哈')
```

















