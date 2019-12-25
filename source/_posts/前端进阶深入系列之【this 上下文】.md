---
title: 前端进阶深入系列之【this上下文】
categories: 
- 前端
- javascript
- 前端深入系列
tags: 
- 前端
- javascript
- 前端深入系列
---

函数的调用 arguments 和 this、函数的调用方式、函数上下文方式！

<!--more-->

### 一、this 和 arguments

#### 1、this 执行上下文

> this 表示被调用函数上下文对象。



#### 2、argumments 参数

> 表示函数调用过程中传递的所有参数。arguments 是个类数组结构，并不能当数组使用 。

```
arguments[2]
```



### 二、函数调用

> **函数的上下文 this 取决于函数的调用方式！**，函数的调用一共四种：

- **直接调用；**
- **对象调用；**
- **构造函数调用（new）；**
- **apply、call 调用；**



#### 1、直接调用

> 直接调用的三种方式：this 的指向，在非严格模式下，this 指向 `window `，在严格模式下，this 指向 `undefined`。

- 函数定义调用；
- 函数表达式调用；
- 立即执行函数调用；



#### 2、对象调用

> 某个对象调用函数，this 将会执行调用的对象，并且可以在函数内部访问到。



#### 3、 构造函数调用

> 通过 new 操作符调用函数，生成一个实例化对象。
>
> **注意：**
>
> 1）如果构造函数返回一个对象，则该对象作为整个表达式的值返回，而传入构造函数的 this 将被丢弃。
>
> 2）如果构造函数返回的是非对象类型，则忽略返回值，返回新创建的对象。

1）创建一个空对象；

2）将该对象作为 this 参数传递给构造函数，称为构造函数的上下文；

3）构造函数将新对象返回。



#### 4、 Call 调用

###### ▉ Call 的内部实现

- 首先 `context` 为可选参数，如果不传的话默认上下文为 `window`；
- 接下来给 `context` 创建一个 `fn` 属性，并将值设置为需要调用的函数；
- 因为 `call` 可以传入多个参数作为调用函数的参数，所以需要将参数剥离出来；
- 然后调用函数并将对象上的函数删除。



**▉ ※ 手写一个 call 方法：**

```javascript
// this 为调用的函数
// context 是参数对象
Function.prototype.myCall = function(context){
    // 判断调用者是否为函数
    if(typeof this !== 'function'){
        throw new TypeError('Error')
    }
    // 不传参默认为 window
    context = context || window
    // 新增 fn 属性,将值设置为需要调用的函数
    context.fn = this 
    // 将 arguments 转化为数组将 call 的传参提取出来  [...arguments]
    const args = Array.from(arguments).slice(1)
    // 传参调用函数
    const result = context.fn(...args)
    // 删除函数
    delete context.fn
    // 返回执行结果
    return result;
}
// 普通函数
function print(age){
    console.log(this.name+" "+age);
}
// 自定义对象
var obj = {
    name:'小鹿'
}
// 调用函数的 call 方法
print.myCall(obj,1,2,3)
```



#### 5、apply 调用

###### ▉ apply 的内部实现

- 首先 `context` 为可选参数，如果不传的话默认上下文为 `window`
- 接下来给 `context` 创建一个 `fn` 属性，并将值设置为需要调用的函数
- 因为 `apply` 传参是数组传参，所以取得数组，将其剥离为顺序参数进行函数调用
- 然后调用函数并将对象上的函数删除



**▉ ※ 手写一个 apply 方法：**

```javascript
// 手写一个 apply 方法
Function.prototype.myApply = function(context){
    // 判断调用者是否为函数
    if(typeof this !== 'function'){
        throw new TypeError('Error')
    }
    // 不传参默认为 window
    context = context || window
    // 新增 fn 属性,将值设置为需要调用的函数
    context.fn = this
    // 返回执行结果
    let result;
    // 判断是否有参数传入
    if(arguments[1]){
        result = context.fn(...arguments[1])
    }else{
        result = context.fn()
    }
    // 删除函数
    delete context.fn
    // 返回执行结果
    return result;
}

// 普通函数
function print(age,age2,age3){
    console.log(this.name+" "+ age + " "+ age2+" "+age3);
}
// 自定义对象
var obj = {
    name:'小鹿'
}
// 调用函数的 call 方法
print.myApply(obj,[1,2,3])
```



#### 4、补充

> 三者可以方便理解为：第一个参数调用了该方法，并将第二个参数作为该方法的参数传入。

**共同点：**

① apply 、 call 、bind 三者都是用来改变函数的 this 对象的指向的；

② apply 、 call 、bind 三者第一个参数都是 this 要指向的对象，也就是想指定的上下文；

③ apply 、 call 、bind 三者都可以利用后续参数传参；

**不同点：**

①  call 顺序传参，而 apply 是数组传参。

② bind 的 `this` 取决于第一个参数，如果第一个参数为空，那么就是 `window`。

③ bind 是返回对应函数（**需要加一对花括号进行调用**），便于稍后调用；apply 、call 则是立即调用 。



### 三、 箭头函数

> 之前的 this 的上下问问题会与预期不否(如：事件处理器)，但是可以通过 apply 、call 绕过。还有另外两种解决办法：

- **箭头函数**
- **bind 函数**



#### 1、箭头函数

> 箭头函数可以规避上下文问题。箭头函数没有单独的 this 值。箭头函数的 this 与声明所在的上下文的 this 相同（调用箭头函数，不会隐士的传入 this 参数，而是从定义时的函数继承上下文）。



###### ▉ 箭头函数和对象字面量

> 1）箭头函数在创建的时候就已经确定了 this 的指向。
>
> 2）字面量中的箭头函数指向全局的 window 对象。



#### 2、bind 函数

###### ▉ bind 的内部实现

- 判断调用者是否为函数。
- 截取参数，注意：这里有两种形式传参。
- 返回一个函数，判断外部哪种方式调用了该函数（new | 直接调用）



###### ▉ ※ 手写一个 bind方法：

```
// 手写一个 bind 函数
Function.prototype.myBind = function (context) {
    // 判断调用者是否为函数
    if(typeof this !== 'function'){
        throw new TypeError('Error')
    }
    // 截取传递的参数
    const args = Array.from(arguments).slice(1)
    // _this 指向调用的函数
    const _this = this;

    // 返回一个函数
    return function F(){
        // 因为返回了一个函数，我们可以 new F()，所以需要判断
        // 对于 new 的情况来说，不会被任何方式改变 this
        if(this instanceof F){
            return new _this(...args,...arguments)
        }else{
            return _this.apply(context,args.concat(...arguments))
        }
    }
}

// 普通函数
function print(){
    // new 的方式调用 bind 参数输出换做 [...arguments]
    console.log(this.name);  
}
// 自定义对象
var obj = {
    name:'小鹿'
}
// 调用函数的 call 方法
let F = print.myBind(obj,1,2,3);
// 返回对象
let obj1 = new F();
console.log(obj1);
```





















