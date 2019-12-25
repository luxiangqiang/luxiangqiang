---
title: 前端 Vue 之内部运行机制【template 模板是怎样通过 Compile 编译的】
categories:
- 前端框架
- Vue
tags:
- 前端框架
- Vue
---

templlate 是如何通过 Compile 编译的？

<!--more-->



## 一、template（模板）

```html
<div :class="c" class="demo" v-if="isShow">
    <span v-for="item in sz">{{item}}</span>
</div>
```



## 二、parse（解析）

> parse 会通过正则等方式将 template 模板进行字符串解析，得到指令、class、style等属性。形成 AST。



### 2.1 正则

> 通过正则进行解析字符串，形成 AST

```
{
    /* 标签属性的map，记录了标签上属性 */
    'attrsMap': {
        ':class': 'c',
        'class': 'demo',
        'v-if': 'isShow'
    },
    /* 解析得到的:class */
    'classBinding': 'c',
    /* 标签属性v-if */
    'if': 'isShow',
    /* v-if的条件 */
    'ifConditions': [
        {
            'exp': 'isShow'
        }
    ],
    /* 标签属性class */
    'staticClass': 'demo',
    /* 标签的tag */
    'tag': 'div',
    /* 子标签数组 */
    'children': [
        {
            'attrsMap': {
                'v-for': "item in sz"
            },
            /* for循环的参数 */
            'alias': "item",
            /* for循环的对象 */
            'for': 'sz',
            /* for循环是否已经被处理的标记位 */
            'forProcessed': true,
            'tag': 'span',
            'children': [
                {
                    /* 表达式，_s是一个转字符串的函数 */
                    'expression': '_s(item)',
                    'text': '{{item}}'
                }
            ]
        }
    ]
}
```



## 三、optimize（优化）

### 3.1 优化的原因

`optimize` 主要用过优化。因为 `patch` 的过程实际上是将 `VNode` 节点进行一层一层的比对，然后将「差异」更新到视图上。其中一些静态的结点没有数据挂载，所以没必要进行对比了。

在静态的结点进行标记，当我们 patch 对比结点的时候，就不用进行对比了，从而达到了优化的目的。

经过 `optimize` 这层的处理，每个节点会加上 `static` 属性，用来标记是否是静态的。



### 3.2 判断静态结点（isStatic）

传入一个 node 判断该 node 是否是静态节点。判断的标准是当 type 为 2（表达式节点）则是非静态节点，当 type 为 3（文本节点）的时候则是静态节点，当然，如果存在 `if` 或者 `for`这样的条件的时候（表达式节点），也是非静态节点。

```javascript
function isStatic (node) {
    if (node.type === 2) {
        return false
    }
    if (node.type === 3) {
        return true
    }
    return (!node.if && !node.for);
}
```



### 3.3 标记静态结点（markStatic）

`markStatic` 为所有的节点标记上 `static`，遍历所有节点通过 `isStatic` 来判断当前节点是否是静态节点，此外，会遍历当前节点的所有子节点，如果子节点是非静态节点，那么当前节点也是非静态节点。

```javascript
function markStatic (node) {
    node.static = isStatic(node);
    if (node.type === 1) {
        for (let i = 0, l = node.children.length; i < l; i++) {
            const child = node.children[i];
            markStatic(child);
            if (!child.static) {
                node.static = false;
            }
        }
    }
}
```



### 3.4 标记静态根节点（markStaticRoots）

接下来是 `markStaticRoots` 函数，用来标记 `staticRoot`（静态根）。这个函数实现比较简单，简单来将就是如果当前节点是静态节点，同时满足该节点并不是只有一个文本节点左右子节点时，标记 `staticRoot` 为 true，否则为 false。

```javascript
function markStaticRoots (node) {
    if (node.type === 1) {
        if (node.static && node.children.length && !(
        node.children.length === 1 &&
        node.children[0].type === 3
        )) {
            node.staticRoot = true;
            return;
        } else {
            node.staticRoot = false;
        }
    }
}
```



### 3.5 实现 optimize

有了以上的函数，就可以实现 `optimize` 了。

```javascript
function optimize (rootAst) {
    markStatic(rootAst);
    markStaticRoots(rootAst);
}
```

转化结果：

```json
{
    'attrsMap': {
        ':class': 'c',
        'class': 'demo',
        'v-if': 'isShow'
    },
    'classBinding': 'c',
    'if': 'isShow',
    'ifConditions': [
        'exp': 'isShow'
    ],
    'staticClass': 'demo',
    'tag': 'div',
    /* 静态标志 */
    'static': false,
    'children': [
        {
            'attrsMap': {
                'v-for': "item in sz"
            },
            'static': false,
            'alias': "item",
            'for': 'sz',
            'forProcessed': true,
            'tag': 'span',
            'children': [
                {
                    'expression': '_s(item)',
                    'text': '{{item}}',
                    'static': false
                }
            ]
        }
    ]
}
```



## 四、generate

`generate` 会将 AST 转化成 render funtion 字符串，最终得到 render 的字符串以及 staticRenderFns 字符串。



















































