---
title: 前端 Vue 之内部运行机制【响应式的基本原理】
categories:
- 前端框架
- Vue
tags:
- 前端框架
- Vue
---

深入响应式的基本原理！

<!--more-->



### 一、响应式系统

Vue.js 是一款 MVVM 框架，数据模型仅仅是普通的 JavaScript 对象，但是对这些对象进行操作时，却能影响对应视图，它的核心实现就是「**响应式系统**」。



### 二、`Object.defineProperty`

> Vue 就是通过 `Object.defineProperty` 实现响应式的。

```javascript
/*
    obj: 目标对象
    prop: 需要操作的目标对象的属性名
    descriptor: 描述符
    return value 传入对象
*/
Object.defineProperty(obj, prop, descriptor)
```



### 三、`observer`观察者模式

通过使用观察者模式，让每个对象变为可观察的。这部分是在 init 初始化的时候进行的，对数据进行【响应式化】。



#### 3.1 声明一个函数 defineReactive

该函数传入的参分别为 obj（要绑定的对象）、key（对象的某一属性）、val（属性值），然后在内部对该对象的属性进行监听（响应式化）。

```javascript
function defineReactive (obj, key, val) {
    Object.defineProperty(obj, key, {
        enumerable: true,       /* 属性可枚举 */
        configurable: true,     /* 属性可被修改或删除 */
        get: function reactiveGetter () {
            return val;         /* 实际上会依赖收集，下一小节会讲 */
        },
        set: function reactiveSetter (newVal) {
            if (newVal === val) return;
            cb(newVal); // 更新视图
        }
    });
}
```



#### 3.2 再封装一层函数

在给上述的函数封装一层函数，参数为要绑定的对象，为了能够在初始化的时候，通过遍历每个对象的属性都要进行响应式化，这个过程其实是一个**递归过程**。

```javascript
function observer (value) {
    if (!value || (typeof value !== 'object')) {
        return;
    }
    
    Object.keys(value).forEach((key) => {
        defineReactive(value, key, value[key]);
    });
}
```



### 四、封装一个 Vue

通过对 Vue 中 data 里边的数据进行处理（实际上 data 是一个函数，这里当做对象来处理）。

```javascript
class Vue {
    /* Vue构造类 */
    constructor(options) {
        this._data = options.data;
        observer(this._data);
    }
}

function observer (value) {
    if (!value || (typeof value !== 'object')) {
        return;
    }
    
    Object.keys(value).forEach((key) => {
        defineReactive(value, key, value[key]);
    });
}

function defineReactive (obj, key, val) {
    Object.defineProperty(obj, key, {
        enumerable: true,       /* 属性可枚举 */
        configurable: true,     /* 属性可被修改或删除 */
        get: function reactiveGetter () {
            return val;         /* 实际上会依赖收集，下一小节会讲 */
        },
        set: function reactiveSetter (newVal) {
            if (newVal === val) return;
            cb(newVal); // 更新视图
        }
    });
}
```

```javascript
let o = new Vue({
    data: {
        test: "I am test."
    }
});
o._data.test = "hello,world.";  // 触发了视图更新
```

















