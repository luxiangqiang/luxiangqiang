---
title: 重学前端之HTML语义1【基本入门】
categories:
- 前端
- HTML
tags:
- 前端
- HTML
---



小鹿带你走进重学前端中的 HTML 语义之基本入门。

<!--more-->



## 一、语义标签

### 1、语义标签是什么？

> 像 section 、nav、p 这样的语义类标签，每个标签都有自己表达的意思。



### 2、为什么要用语义标签？

- 增强可读性，没有 css  也能清晰地看出网页的结构，便于团队维护和开发。
- 适合于机器阅读，文字表现力丰富，更适合于搜索引擎检索（SEO），有效提升网页的搜索量。
- 语义类标签支持读屏软件，根据文章可以自动生成目录等等。



### 3、怎么用语义标签？

> 1、“用对”比“不用”好，“不用” 比 “用错” 好，我们要不断的追求“用对”。
>
> 2、错误的语义标签会造成阅读的混淆、增加嵌套，给 CSS 编写加重负担。



### 4、自然语言语义便签

#### <em></em>

> 表示强调某个关键词。



**Example**

```html
今天我吃了一个 <em> 苹果 </em>。
今天我吃了 <em> 一个 </em> 苹果。
```



### 5、标题摘要语义标签

#### ■ hgroup 标签

> **① 代码实例**

```
例如：

<h1>HTML 语义 </h1>
<p>balah balah balah balah</p>
<h2> 弱语义 </h2>
<p>balah balah</p>
<h2> 结构性元素 </h2>
<p>balah balah</p>
......

```



> **② 树形结构**

- HTML 语义 
  - 弱语义 
  - 结构性元素
  -  ……



> **③ 缺点**

h1 - h6 代表文章不同层次的标题，如果我们有副标题，也会用到 h1 - h6 那我们怎么办呢？



> **④ 改进**

```html
<hgroup>
    <h1>JavaScript 对象 </h1>
    <h2> 我们需要模拟类吗？</h2>
</hgroup>
<p>balah balah</p>
......
```



> **⑤ 树形目录**

- JavaScript 对象——我们需要模拟类吗？ 
  - …



#### ■ section 标签

> section 不仅仅是一个 “ 有语义的 div ”，还会改变 h1 - h6  的语义。，他会使得其中的 h1 - h6 下降一级，所以只需要 section 和 h1 就能形成文档树形结构。



例子

```html
<section>
    <h1>HTML 语义 </h1>
    <p>balah balah balah balah</p>
    <section>
        <h1> 弱语义 </h1>
        <p>balah balah</p>
    </section>
    <section>
        <h1> 结构性元素 </h1>
        <p>balah balah</p> 
    </section>
......
</section>
```



- HTML 语义 
  - 弱语义 
  - 结构性元素 
  - ……



### 6、作为整体结构的语义标签

> 很多浏览器推出「阅读模式」，以及非浏览器终端的出现，语义化的 HTML 适合机器阅读特性变的越来越重要。



实例

```html
<body>
    <header>
        <nav>
            ……
        </nav>
    </header>
    <aside>
        <nav>
            ……
        </nav>
    </aside>
    <section>……</section>
    <section>……</section>
    <section>……</section>
    <footer>
        <address>……</address>
    </footer>
</body>

```



#### article 标签

> 一个典型的场景是多篇新闻展示在同一个新闻专题页面中，这种类似报纸的多文章结构适合用 article 来组织。



例子：

```html
<body>
    <header>……</header>
    //文章一
    <article>
        //头
        <header>……</header>
        //主体
        <section>……</section>
        <section>……</section>
        <section>……</section>
        //尾
        <footer>……</footer>
    </article>
    //文章二
    <article>
        ……
    </article>
    <article>
        ……
    </article>
    <footer>
        <address></address>
    </footer>
</body>

```



#### header 标签

> 通常出现在前部，表示导航或者介绍性的内容。



#### footer 标签

> 通常出现在尾部，包含一些作者信息、相关链接、版权信息等。



#### aside 标签

> 跟文章主体不那么相关的部分，它可能包含导航、广告等工具性质的内容。**「侧边栏是 aside，aside 不一定是侧边栏。」**



#### aside 和 header 中的导航（nav）区别？

> 1、header 中的导航多数是到文章自己的**目录**。
>
> 2、aside 中的导航多数是到**关联页面**或者是**整站地图**。



#### footer 中 address 标签

> 表示文章（作者）的**联系方式**。



























