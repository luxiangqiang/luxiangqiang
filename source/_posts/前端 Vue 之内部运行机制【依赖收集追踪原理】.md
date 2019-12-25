---
title: 前端 Vue 之内部运行机制【依赖收集追踪原理】
categories:
- 前端框架
- Vue
tags:
- 前端框架
- Vue
---

响应式系统的依赖收集追踪原理！

<!--more-->



### 一、为什么要进行依赖收集？

#### 1、没有渲染的变量

通常 `data` 中的数据我们会在 `template` 中进行渲染，有些 data 中的数据使用不到，所以在我们更改这些使用不到的数据的时候，没必要进行更新视图了。



#### 2、多个使用的实例

如果一个 data 数据对象关联多个 vue 实例的话，data 数据改动，「依赖收集」会让这个数据知道有几个地方依赖数据，在变化的时候通知他们。

最终会形成数据与视图的一种对应关系，如下图。

![img](https://user-gold-cdn.xitu.io/2018/1/5/160c4572fdd738f2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 二、依赖收集是怎么实现的？

#### 订阅者 (Dep) 

> 实现一个订阅者 Dep，主要作用用来存放 Watcher 观察者对象。

1. 用 `addSub` 方法可以在目前的 `Dep` 对象中增加一个 `Watcher` 的订阅操作；
2. 用 `notify` 方法通知目前 `Dep` 对象的 `subs` 中的所有 `Watcher` 对象触发更新操作。

```javascript
class Dep {
    constructor () {
        /* 用来存放Watcher对象的数组 */
        this.subs = [];
    }

    /* 在subs中添加一个Watcher对象 */
    addSub (sub) {
        this.subs.push(sub);
    }

    /* 通知所有Watcher对象更新视图 */
    notify () {
        this.subs.forEach((sub) => {
            sub.update();
        })
    }
}
```



#### 观察者 (Watcher) 

> 主要用来关联对象，更新视图。

```javascript
class Watcher {
    constructor () {
        /* 在new一个Watcher对象时将该对象赋值给Dep.target，在get中会用到 */
        Dep.target = this;
    }

    /* 更新视图的方法 */
    update () {
        console.log("视图更新啦～");
    }
}

Dep.target = null;
```



#### 依赖收集

我们在闭包中增加了一个 Dep 类的对象，用来收集 `Watcher` 对象。在对象被「读」的时候，会触发 `reactiveGetter` 函数把当前的 `Watcher` 对象（存放在 Dep.target 中）收集到 `Dep` 类中去。之后如果当该对象被「**写**」的时候，则会触发 `reactiveSetter` 方法，通知 `Dep` 类调用 `notify` 来触发所有 `Watcher` 对象的 `update` 方法更新对应视图。

```javascript
class Vue {
    constructor(options) {
        this._data = options.data;
        observer(this._data);
        /* 新建一个 Watcher 观察者对象，这时候 Dep.target 会指向这个 Watcher 对象 */
        new Watcher();
        /* 在这里模拟 render 的过程，为了触发 test 属性的 get 函数 */
        console.log('render~', this._data.test);
    }
}

// 管理监听 Watcher 对象
class Dep {
    constructor () {
        /* 用来存放Watcher对象的数组 */
        this.subs = [];
    }

    /* 在subs中添加一个Watcher对象 */
    addSub (sub) {
        this.subs.push(sub);
    }

    /* 通知所有Watcher对象更新视图 */
    notify () {
        this.subs.forEach((sub) => {
            sub.update();
        })
    }
}

// 观察者更新视图
class Watcher {
    constructor () {
        /* 在new一个Watcher对象时将该对象赋值给Dep.target，在get中会用到 */
        Dep.target = this;
    }

    /* 更新视图的方法 */
    update () {
        console.log("视图更新啦～");
    }
}

Dep.target = null;

// 响应系统关联对象
function observer (value) {
    if (!value || (typeof value !== 'object')) {
        return;
    }
    
    Object.keys(value).forEach((key) => {
        defineReactive(value, key, value[key]);
    });
}

// 关联更新视图
function defineReactive (obj, key, val) {
    /* 一个Dep类对象 */
    const dep = new Dep();
    
    Object.defineProperty(obj, key, {
        enumerable: true,
        configurable: true,
        get: function reactiveGetter () {
            /* 将 Dep.target（即当前的 Watcher 对象存入 dep 的 subs 中） */
            dep.addSub(Dep.target);
            return val;         
        },
        set: function reactiveSetter (newVal) {
            if (newVal === val) return;
            /* 在set的时候触发dep的notify来通知所有的Watcher对象更新视图 */
            dep.notify();
        }
    });
}
```

- 在 `observer` 的过程中会注册 `get` 方法，该方法用来进行「**依赖收集**」—— 「**依赖收集**」的过程就是把 `Watcher` 实例存放到对应的 `Dep` 对象中去。
- `get` 方法可以让当前的 `Watcher` 对象（Dep.target）存放到它的 subs 中（`addSub`）方法
- 在数据变化时，`set` 会调用 `Dep` 对象的 `notify` 方法通知它内部所有的 `Watcher` 对象进行视图更新。



「**依赖收集**」的前提条件还有两个：

- 触发 `get` 方法 —— 实际上只要把 `render function` 进行渲染时候就会读取 get 方法。
- 新建一个 `Watcher` 对象。



结合依赖收集和响应式原理看整个过程：

![img](https://user-gold-cdn.xitu.io/2017/12/19/1606edad5ca9e23d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

































