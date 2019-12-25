---
title: 前端进阶深入系列之【代理Proxy】
categories: 
- 前端
- javascript
- 前端深入系列
tags: 
- 前端
- javascript
- 前端深入系列
---

前端深入系列之控制对象的访问！

<!--more-->



###  一、getter 和 setter 的作用?

> 1）避免错误赋值；
>
> 2）记录属性的变化；
>
> 3）数据绑定



### 二、使用 getter 和 setter  方式

#### 1、对象字面量定义

> 1）对象访问属性通常隐士调用 getter 方法。
>
> 2）系统自动关联 getter 和 setter 方法。
>
> 3）通过 getter 和 setter 中记录访问和控制日志。

```javascript
// 字面量定义 setter 和 getter
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
console.log(collection.name)
```



#### 2、Class 定义

> 在 ES6 中不能只单独的指定 getter 和 setter，在非严格模式下不起作用，在严格模式下会抛出异常。所以下方出现了 Object.defineProperty 方法。

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



#### 3、Object.defineProperty

> 1）对象的字面量与类、getter 和 setter 方法不是在同一作用域中定义的，所以设置私有变量也是无法实现的。所以出现了 Object.defineProperty 方法。
>
> 2）Object.defineProperty 创建的 get 和 set 方法与私有变量处于相同的作用域，set 和 get 方法分别创建了含有私有变量的闭包。

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
const xiaolu1 = new xiaolu();
console.log(xiaolu1.skillLevel)
```



### 三、getter 和 setter 的应用

#### 1、数据校验

> 验证传入的值是否为整型，如果不是则抛出异常。

```javascript
 function xiaolu(){
     let count = 0;
     Object.defineProperty(this,'skillLevel',{
         get: () => {
             console.log("get")
             return count;
         },
         set: value => {
             if(!Number.isInteger(value)){
                 throw new TypeError("skill level")
             }
             count = value;
         }
     })
 }
const xiaolu1 = new xiaolu();
console.log(xiaolu1.skillLevel = 1)
```



#### 2、计算属性值

> 每次访问我们都会对属性值进行处理。



### 四、Proxy（代理）

#### 1、Proxy 的使用

> 通过 Proxy 构造器创建代理对象，代理对象访问目标对象时执行指定的操作。
>
> 1）target：传入的对象;
>
> 2）key：属性名称;
>
> 3）value：属性值。

```javascript
// 对象代理
const emperor = {name:'xiaolu',age:20}
const representative = new Proxy(emperor,{
    get:(target,key)=>{
        console.log(target)
        return key in target ? target[key] : '没有该属性'
    },
    set:(target,key,value)=>{
        console.log(target)
        console.log(value)
    }
})
console.log(representative.a) // 没有该属性
```



#### 2、Proxy 的优点

> 1）每个 getter 和  setter 仅只能控制单个对象属性，Proxy 方法可以控制对象的多个属性。
>



#### 3、代理的应用

###### ▉ 应用一：代理记录日志

> 代理的直接用途之一就是读写属性时使用一种更好、更清洁的方式启动日志记录。

优点：

- 不会把原有代码和日志混淆，也不需要为每个对象属性添加单独的日志。
- 所有的读写属性的操作都会进入代理的方法。
- 日志代码只需指定一处，无论读写属性多少次、无论属性增加多少，都可以记录对应的日志。



###### ▉ 应用二：代理检测性能

> 1）代理可以在不修改函数代码的情况下，可以评估函数调用的性能。
>
> 2）为代理对象添加 apply 方法，当代理的函数被调用时，就会调用 apply 方法。
>
> 3）参数:
>
> - target: 调用的函数本身;
> - args：函数传入的参数。

```javascript
function isPrime(number){
    if(number < 2){return false;}
    for(let i = 2;i < number;i++){
        if(number % i === 0){return false;}
        return true
    }
}

isPrime = new Proxy(isPrime,{
    apply:(target, thisArg, args) =>{
        // 开始计时
        console.time("isPrime")
        const result = target.apply(args)
        // 计时结束，并输出执行时间
        console.timeEnd("isPrime")
        // 返回执行结果
        return result;
    }
}) 

isPrime(1299827)
```



###### ▉ 应用三：代理自动填充属性

> 如果属性不存在，则创建该属性。



###### ▉ 应用四：代理实现负数组索引



#### 4、代理的性能消耗

> 1）虽然代理可以定义执行特定的操作就会调用特定的方法，但是所有的操作加了一个间接层，与此同时引入了大量的额外处理，会影响性能问题。
>
> 2）单个执行操作发生的非常快，难以衡量代理的可靠性，需要经过多次执行代码检测。
>
> 3）建议谨慎使用代理，大量的控制对象的访问会带来性能上的问题。

















