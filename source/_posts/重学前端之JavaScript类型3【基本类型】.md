---
title: 重学前端之JavaScript类型2【基本类型】
categories:
- 前端
- js
tags:
- 前端
- js
---



 小鹿带你走进重学前端中的基本类型！

<!--more-->





## javascript 基本类型

> **运行时类型**是代码实际执行过程中我们用到的类型。所有的类型数据都会属于 7 个类型之一。从变量、参数、返回值到表达式中间结果，任何 JavaScript 代码运行过程中产生的数据，都有运行时类型。



### 一、七大基本类型

- Undefined;
- null;
- Boolean;
- String;
- Number;
- Symbol;
- Object;



#### ■ Undefined、Null

**① Undefined**

MDN 官网解释： 

> A **primitive** value automatically assigned to **variables** that have just been declared or to **formal arguments** for which there are no **actual arguments**。

Undefined 表示未定义，只有一个值为 undefined。



> 问题：为什么编程规范用 void（0）来代替 undefind ？

**答：**因为 undefined 是一个变量，并非是一个关键字，这是 js  语言公认的设计失误之一，所以为了避免被篡改，所以建议用 void(0) 代替它。



**② Null**

> Null 值为一个空对象的指针，用 type 检测类型为 object 类型。Null 为值定义了，但是为空，是 js 中的关键字。



**③ undefined/null 的区别**

> 1、undefined 值是派生自 null 的值，所以 alter(undefined == null) // true。
>
> 2、声明的变量没有必要显示的赋值 undefined ,而 null 可以将即将保存的对象的变量赋值为 null。



#### ■ String

> **字符串类型：**String 最大长度为 2^53 -1 。所谓的字符串长度并不是字符数，而是字符串的 uft16 编码的影响的。



#### ■ Number 

**①  区分 +0 和 -0 。**

> 加法运算没有区别，但是除法计算注意，除以 -0 会得到 -Infinity 从而导致错误。用 1/x 来检测 Infinity 或 -Infinity。



**② 为什么 0.1+0.2 不等于 0.3。**

> 非整数类型不能用 == 或者 === 来比较。之所以上方不相等原因就是浮点型的预算精度问题，导致不是严格相等，只是相差了微小的值。



**③ 检测浮点型的方法（最小精度）**

> J检查等式左右两边的差的绝对值是否小于最小精度来比较浮点数。

```javascript
  console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON); //true
```



#### ■ symbol

> Symbol 是 ES6 中引入的新类型，它是一切非字符串的对象 key 的集合。



**① 创建 Symbol 变量。**

```javascript
    var mySymbol = Symbol("my symbol"); //使用全局的 symbol 函数来创建
```



#### ■ Object

**① 让对象的方法用在基本类型上。**

> 运算符提供了装箱操作，它会根据基础类型构造一个临时对象。

```
    cosole.log("abc".charAt(0)); //a
```

**② 在原型方法上添加方法和属性。**

```
    Symbol.prototype.hello = () => console.log("hello");

    var a = Symbol("a");
    console.log(typeof a); //symbol，a 并非对象
    a.hello(); //hello，有效
```



###  二、类型转换

> “==” 运算是 js 设计的失误，“==”  运算试图跨类型比较，规则非常的复杂，很多实际中禁止使用 “==” 运算。我们通常进行显示的转换之后，用 “===” 来进行比较。



#### ■ StringToNumber

> 字符串到数字类型的转换。，类型转换支持十进制、二进制、八进制和十六进制。



**注意：**

> parseInt 和 parseFloat 转换语法与这里的转换不尽相同。
>
> 1、parseInt  不传入第二个参数，只支持 16 进制的转换，会忽略非数字和科学计数法。
>
> 2、parseFloat  直接将原字符串作为十进制来解析。



#### ■ NumberToString

> 1、在较小的范围内，数字到字符串的转换是完全符合你直觉的十进制表示。2
>
> 2、在Number 绝对值较大时，字符串是用科学计数法来表示的。（实际用途不大）



#### ■ 装箱转换

> 装箱转换就是把基本类型转换成对应的对象类型。



**1、Symbol 不能用 new 来调用，我们借助 call 方法强制装箱。**

```javascript
    var symbolObject = (function(){ return this; }).call(Symbol("a"));

    console.log(typeof symbolObject); //object
    console.log(symbolObject instanceof Symbol); //true
    console.log(symbolObject.constructor == Symbol); //true
```

> **注意：**装箱机制会产生临时对象，在一些性能要求较高的场景下，尽量避免对基本类型做装箱操作。



**2、使用 object 进行显示调用装箱操作**

```javascript
    var symbolObject = Object((Symbol("a"));

    console.log(typeof symbolObject); //object
    console.log(symbolObject instanceof Symbol); //true
    console.log(symbolObject.constructor == Symbol); //true
```



#### ■ 拆箱操作

> 将对象类型到基本类型的转换。
>
> 对象 string 和 Number 的转换遵循 “先拆箱再转换” 的规则，通过拆箱装换，把对象变成基本基本类型，再从基本类型转换为 Number 或者 String。



1、拆箱操作会尝试着调用 valueof 方法和 toString 方法，如果都不存在，则没有基本类型，就会产生类型错误 TypeError。

```javascript
    var o = {
        valueOf : () => {console.log("valueOf"); return {}},
        toString : () => {console.log("toString"); return {}}
    }

    o * 2
    //执行顺序
    // valueOf
    // toString
    // TypeError
```

```javascript
    var o = {
        valueOf : () => {console.log("valueOf"); return {}},
        toString : () => {console.log("toString"); return {}}
    }

    o + ""
    //执行顺序
    // toString
    // valueOf
    // TypeError
```













