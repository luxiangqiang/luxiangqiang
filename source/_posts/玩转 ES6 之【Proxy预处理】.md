---
title:  玩转 ES6 之【对象代理】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

了解对象的访问以及监控对象的技术！

<!--more-->

- 通过 getter 与 setter 访问属性值有什么好处？-
- 代理与 getter 和 setter 的主要区别是什么？
- 代理对象常见的问题是什么？



### 一、为什么需要 getter 和 setter 访问？

1、避免意外的错误发生；

2、需要记录属性的变化；

3、数据绑定。 



### 二、定义 getter 和 setter

> 三种定义方法：

- 字面量定义
- ES6 中的 Class 定义
- 使用 Object.definedProperty 方法



#### 1、字面量定义 getter 和 setter

> 对象访问属性通常隐士调用 getter 和 setter 方法，属性自动关联 getter 和 setter 方法。这些都是标准方法，在访问属性的时候都是立即执行的。

```javascript
const collection = {
	name='xiaolu',
	
	// 读取属性
	get name(){
		return this.name;
	}
	// 设置属性
	set name(value){
		this.name = value;
	}
}
collection.name 			// 隐士调用 getter 方法
collection.name = "xiaolu1" // 隐士调用 setter 方法
```



#### 2、ES6 Class 定义 getter 和 setter

```javascript
// calss 定义 setter 和 getter
class Xiaolu {
    constructor(){
        this.name = 'xiaolu'
    }
	
    get firstXiaolu(){
        console.log('属性已访问')
        return this.name;
    }
	
    set firstXiaolu(value){
        this.name = value;
    }
}

const x = new Xiaolu();
x.firstXiaolu
```



#### 3、Object.definedProperty()

> 原因：对象的字面量与类、getter 和 setter 方法不是在一同一作用域定义的，因此那些希望作为私有变量属性的标量是无法实现的。

```javascript
// Object.definedProperty
function xiaolu(){
    let count = 0;
	
    Object.defineProperty(this,'skillLevel',{
        get:() => {
            return count;
        },
        set:value => {
            count = value;
        }
    })
}
// 隐士的调用 get 方法
console.log(xiaolu.count)
```

> 通过 Object.definedProperty() 创建的 get 和 set 方法，与私有的变量处于相同的作用域中，get 和 set 方法分别创建了含有私有变量的闭包。



### 三、getter 和 setter 的应用

#### 1、日志记录

> 当访问属性时，可以在 getter 和 setter 中记录访问日志。



#### 2、校验值

> 有效的避免指定属性类型错误的发生。

```javascript
function xiaolu(){
    let count = 0;
	
    Object.defineProperty(this,'skillLevel',{
        get:() => {
            return count;
        },
        set:value => {
            if(!Number.isInteger(value)){
                throw new TypeError("抛出错误")
            }
            count = value;
        }
    })
}
```



#### 3、定义如何计算属性值

> 每次访问属性值，都会进行计算属性值。

```javascript
const collection = {
    name:'xiaolu',
    age:'2',
    // 读取属性
    get getName(){
        return this.name + " "+this.age;
    },
    // 设置属性
    set setName(value){
        this.name = value;
    }
}

console.log(collection.getName)
```



### 四、Proxy





















