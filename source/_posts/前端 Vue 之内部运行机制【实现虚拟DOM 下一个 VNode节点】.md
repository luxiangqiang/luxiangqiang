---
title: 前端 Vue 之内部运行机制【实现 Virtual DOM 下的一个 VNode 节点】
categories:
- 前端框架
- Vue
tags:
- 前端框架
- Vue
---

实现虚拟 DOM(Virtual DOM) 的虚拟节点（VNode）!

<!--more-->



### 一、什么是VNode（虚拟节点）？

`render function` 会被转换为 `VNode` 节点(函数的返回结果就是 VNode 类的实例对象)。`Virtual DOM` 其实就是一棵以 `JavaScript` 对象（`VNode` 节点）作为基础的树,就是真实 `DOM` 的抽象，不依赖于真实的平台环境，可以实现跨平台的能力。



### 二、如何实现 Vnode 虚拟节点？

VNode 就是一个 JavaScript 对象，用来描述当前结点的信息。



#### 2.1 模板组件

```html
<template>
  <span class="demo" v-show="isShow">
    This is a span.
  </span>
</template>
```



#### 2.2 生成 VNode 的类

```javascript
class VNode {
    constructor (tag, data, children, text, elm) {
        /*当前节点的标签名*/
        this.tag = tag;
        /*当前节点的一些数据信息，比如props、attrs等数据*/
        this.data = data;
        /*当前节点的子节点，是一个数组*/
        this.children = children;
        /*当前节点的文本*/
        this.text = text;
        /*当前虚拟节点对应的真实dom节点*/
        this.elm = elm;
    }
}
```



#### 2.2  Render function 函数表示

```javascript
function render () {
    return new VNode(
        'span',
        {
            /* 指令集合数组 */
            directives: [
                {
                    /* v-show指令 */
                    rawName: 'v-show',
                    expression: 'isShow',
                    name: 'show',
                    value: true
                }
            ],
            /* 静态class */
            staticClass: 'demo'
        },
        [ new VNode(undefined, undefined, undefined, 'This is a span.') ]
    );
}
```



#### 2.3 Render 函数转化为 VNode 虚拟节点

```json
{
    tag: 'span',
    data: {
        /* 指令集合数组 */
        directives: [
            {
                /* v-show指令 */
                rawName: 'v-show',
                expression: 'isShow',
                name: 'show',
                value: true
            }
        ],
        /* 静态class */
        staticClass: 'demo'
    },
    text: undefined,
    children: [
        /* 子节点是一个文本VNode节点 */
        {
            tag: undefined,
            data: undefined,
            text: 'This is a span.',
            children: undefined
        }
    ]
}
```

















