---
title:  玩转 ES6 之【Generator生成器】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

使用 ES6 的生成器可以优雅的实现异步操作！

<!--more-->



### 一、生成器函数

> 1、使用生成器函数可以生成一组值的序列，每个值的生成是基于每次请求的，并不同于标准函数立即生成。
>
> 2、调用生成器不会直接执行，而是通过叫做**迭代器**的对象控制生成器执行。

```javascript
function* WeaponGenerator(){
    yield "1";
    yield "2";
    yield "3";
}

for(let item of WeaponGenerator()){
    console.log(item);
}
//1
//2
//3
```



### 二、使用迭代器控制生成器

#### 2.1 迭代器的使用

1、通过调用生成器返回一个迭代器对象，用来控制生成器的执行。

2、调用迭代器的 next 方法向生成器请求一个值。

3、请求的结果返回一个对象，对象中包含一个 value 值和 done 布尔值，告诉我们生成器是否还会生成值。

4、如果没有可执行的代码，生成器就会返回一个 undefined 值，表示整个生成器已经完成。

```javascript
function* WeaponGenerator(){
    yield "1";
    yield "2";
    yield "3";
}

let weapon = WeaponGenerator();
console.log(weapon.next());
console.log(weapon.next());
console.log(weapon.next());
```



#### 2.2 生成器的状态

1、每当代码执行到 yield 属性，就会生成一个中间值，返回一个对象。

2、每当生成一个值后，生成器就会非阻塞的挂起执行，等待下一次值的请求。

3、再次调用 next 方法，将生成器从挂起状态唤醒，中断执行的生成器从上次离开的位置继续执行。

4、直到遇到下一个 yield ，生成器挂起。

5、 当执行到没有可执行代码了，就会返回一个结果对象，value 的值为 undefined , done 的值为 true，生成器执行完成。



#### 2.3 将执行权交给下一个生成器

```javascript
function* XiaoLuGenerator(){
    yield "4";
    yield "5";
    yield "6";
}

function* WeaponGenerator(){
    yield "1";
    yield "2";
    yield "3";
    yield *XiaoLuGenerator(); // 将执行权交给另一个生成器
}

for(let item of WeaponGenerator()){
    console.log(item);
}
```



### 三、生成器的应用

#### 1、为对象赋唯一 ID 标识

> 当创建某些对象时，需要为对象赋一个唯一的 ID 值。通常使用一个全局计时器变量，但是这种写法很容易使代码变的混乱。所以使用生成器可以实现这个功能。

```javascript
 function* IdGenerator(){
     let id = 0;
     while (true){
         yield ++id;
     }
 }

const idIterator = IdGenerator();
const obj1 = { id: idIterator.next().value }
const obj2 = { id: idIterator.next().value }
const obj3 = { id: idIterator.next().value }
const obj4 = { id: idIterator.next().value }
const obj5 = { id: idIterator.next().value }
```



#### 2、遍历 DOM 树

> 通常遍历 DOM 树最简单的方法是使用递归，但是使用生成器也可以进行遍历代码。很多情况下，使用迭代器比使用递归更要自然。

```html
<div id="xiaolu">
    <form>
        <input type="text">
    </form>
    <p>Paragraph</p>
    <span>Spam</span>
</div>
```

```javascript
function* DomTraversal(element){
    // 1、打印当前元素
    yield element;
    // 2、寻找当前元素的子元素
    element = element.firstElementChild;
    // 3、循环遍历子元素（子元素可能多个）
    while (element) {
        // 4、子元素可能还有子元素（递归）
        yield* DomTraversal(element);
        // 5、遍历兄弟元素的子元素
        element = element.nextElementSibling;
    }
}

const subTree = document.getElementById("xiaolu");
for(let node of DomTraversal(subTree)){
    console.log(node)
}
```



### 四、生成器数据交互

> 生成器中可以进行双向通信，通过 yield 可以返回值，也可以通过 next 传入值。
>
> **注意：**如果没有等待的 yield 表达式，也就是没有值可以应用，所以第一次的 yield 无法传值。



#### 1、构造函数的初始化

> 生成器可以像其他函数一样接受标准的参数，并在生成器内使用。

```javascript
function* XiaoLuGenerator(action){
    const n = yield ("4"+action);
}

let weapon = XiaoLuGenerator("3333");
console.log(weapon.next());
```



#### 2、next 方法传递值

> next 传递的参数是作为上一执行的结果返回。

```javascript
function* XiaoLuGenerator(action){
    const n = yield ("4"+action);
    yield ("next" + n)
}

let weapon = XiaoLuGenerator("3333");
console.log(weapon.next());
console.log(weapon.next("嘿嘿"));
// {value: "43333", done: false}
// {value: "next嘿嘿", done: false}
```



#### 3、抛出异常

> 生成器除了有一个 next 方法，还有一个 throw 方法来抛出异常，当生成器内部发生错误时，我们可以通过抛出异常来抛出错误。抛出的错误就会被 try-catch 捕获。

```javascript
function* XiaoLuGenerator(){
    try{
        const n = yield "4";
    }catch(e){
        console.log("抛出错误！")
    }
}

let weapon = XiaoLuGenerator("3333");
weapon.next()
weapon.throw("错误!")
```



### 五、生成器的内部结构

> 生成器更像是一个状态运动的状态机。

- 挂起开始状态——创建一个生成器处于未执行状态。
- 执行状态——生成器的执行状态。
- 挂起让渡状态——生成器执行遇到第一个 yield 表达式。
- 完成状态——代码执行到 return 全部代码就会进入全部状态。



#### 1、执行上下文跟踪生成器函数

```javascript
function* WeaponGenerator(action){
    yield "1"+action;
    yield "2";
    yield "3";
}

let Iterator = WeaponGenerator("xiaolu");
let result1 = Iterator.next()
let result2 = Iterator.next()
let result3 = Iterator.next()
```

执行过程：

① 在调用生成器之前的状态——只有全局执行上下文，全局环境中除了生成器变量的引用，其他的变量都为 undefined。

② 调用生成器并没有执行函数，而是返回一个 Iterator 迭代器对象并指向当前生成器的上下文。

③ 一般函数调用完成上下文弹出栈，然后被摧毁。当生成器的函数调用完成之后，当前生成器的上下文出栈，但是在全局的迭代器对象还与保持着与生成器执行上下文引用，且生成器的词法环境还存在。

④ 执行 next 方法，一般的函数会重新创建执行上下文。而生成器会重新激活对应的上下文并推入栈中（这也是为什么标准函数重复调用时，重新从头执行的原因所在。与标准函数相比较，生成器暂时会挂起并将来恢复）。

⑤ 当遇到 yield 关键字的时候，生成器上下文出栈，但是迭代器还是保持引用，处于非阻塞暂时挂起的状态。

⑥ 如果遇到 next 指向方法继续在原位置继续 执行，直到遇到 return 语句，并返回值结束生成器的执行，生成器进入结束状态。

|  执行上下文栈  | 当前环境 | 变量                                                         |
| :------------: | :------: | ------------------------------------------------------------ |
| 全局执行上下文 | 全局环境 | WeaponGenerator:function*(){}<br />Iterator: undefined<br />result1: undefined<br />result2: undefined<br />result3: undefined |
从未执行状态变为执行状态：

|         执行上下文栈         |         当前环境          | 变量                                                         |
| :--------------------------: | :-----------------------: | ------------------------------------------------------------ |
|        全局执行上下文        |         全局环境          | WeaponGenerator:function*(){}<br />Iterator: Iterator<br />result1: undefined<br />result2: undefined<br />result3: undefined |
| WeaponGenerator <br />上下文 | WeaponGenerator<br />环境 | action: xiaolu                                               |











































