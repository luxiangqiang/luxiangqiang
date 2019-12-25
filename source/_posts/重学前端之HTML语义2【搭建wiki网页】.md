---
title: 重学前端之HTML语义2【呈现 wiki 网页】
categories:
- 前端
- HTML
tags:
- 前端
- HTML
password: wiki
---

![](/images/重学前端之HTML语义.png)

小鹿带你走进重学前端之 HTML 语义2 【呈现 wiki 网页】。关注公众号「一个不甘平凡的码农」回复 “博客秘钥”，即可获取文章密码。

<!--more-->



## 如何用 HTML 语义呈现 WIKI 页面？

![](/images/WIKI网页.png)

### 一、网页标签组成



■ aside 侧边栏标签

> 左边「侧边栏」根据上一篇的语义定义，属于 aside 内容，是 导航性质的内容。



■ article 文章标签

> 右侧部分是「文章主题内容」，因为主题部分需要明确的独立性，所以用 article 来包裹。



■ hgroup, h1 , h2 主题标签

> 文章主题上边的「主标题和副标题」应用 hgroup 标签内分别用 h1 和 h2 语义来呈现。

```html
<hgroup>
<h1>World Wide Web </h1>
<h2>From Wikipedia, the free encyclopedia</h2>
</hgroup>
```



■ abbr 缩写标签

> abbr 标签表示缩写。正文中的 “www” 表示 World Wide Web 的缩写，所以我们用 abbr 标签。

```html
<abbr title="World Wide Web">WWW</abbr>.
```



■ hr 横线标签

> hr 标签并不仅仅所有的横线就用 hr 标签，**hr 的语义是故事的走向或者话题的转变**，此处两个标题并非这种关系，所以使用 css 中的 border 表示。



■ p 段落标签

> 下方的文章段落使用的是 p 标签，段落标签。



■ strong 黑体标签

> 文中出现很多黑体字，我们用 strong 标签进行标记。

```html
<p> 
A global map of the web index for countries in 2014
<strong>The World Wide Web (WWW)</strong>, also called <strong>the Web</strong>,
......
```



■ blockquote ,q ,cite 引述标签

> **blockquote** ：表示段落引述内容。
>
> **q** : 表示行内的引述内容。
>
> **cite** : 表示引述的作品名。

> 这里的是作品名称，所以用 cite 来引述。

```html
<cite>"What is the difference between the Web and the Internet?"</cite>. W3C Help and FAQ. W3C. 2009. Archived from the original on 9 July 2015. Retrieved 16 July 2015.
```



■ time 日期标签

> 为了让机器阅读更加方便，我们加上 time 标签。

```html
<cite>"What is the difference between the Web and the Internet?"</cite>. W3C Help and FAQ. W3C. 2009. Archived from the original on <time datetime="2015-07-09">9 July 2015</time>. Retrieved <time datetime="2015-07-06">16 July 2015</time>.
```



■ figure , figcaption 标签

> 右侧的文字加图片组成 figure 的语法现象。

```html
<figure>
     <img src="https://.....440px-NeXTcube_first_webserver.JPG"/>
     <figcaption>The NeXT Computer used by Tim Berners-Lee at CERN.</figcaption>
</figure>
```



■ dfn 被定义标签

> dfn 标签是用来包裹被定义的名词。
>

```html
//Internet是一个由相互连接的计算机网络组成的全球系统。
The <dfn>Internet</dfn> is a global system of interconnected computer networks.
// 与此相反，万维网是全球的文件和资源管理的集合。
In contrast, the <dfn>World Wide Web</dfn> is a global collection of documents and 
```



■ nav , ol ,ul 列表导航标签

> 下方文章的目录我们可以用 nav 来加 ol 有序列表来实现。

```html
<nav>
  <h2>Contents</h2>
  <ol>
    <li><a href="...">History</a></li>
    <li><a href="...">Function</a>
      <ol>
        <li><a href="...">Linking</a></li>
        <li><a href="...">Dynamic updates of web pages</a></li>
        ...
      </ol>
    </li>
    ...
  </ol>
</nav>
```



■  pre, samp, code 代码或预先编译标签

> **pre ：**有时候我们并不需要 html 的自动换行，所以我们使用 pre 标签能表示这部分内容是预先排版过的，不需要浏览器重新排版。
>
> **samp : **一段计算机的示例输出，所以我们可以使用 samp 标签。
>
> **code :**用来显示 HTML 标签代码。

示例：

```html
<html>
  <head>
    <title>Example.org – The World Wide Web</title>
  </head>
  <body>
    <p>The World Wide Web, abbreviated as WWW and commonly known ...</p>
  </body>
</html>
```

代码实现：

```html
<pre><code>
&lt;html&gt;
  &lt;head&gt;
    &lt;title&gt;Example.org – The World Wide Web&lt;/title&gt;
  &lt;/head&gt;
  &lt;body&gt;
    &lt;p&gt;The World Wide Web, abbreviated as WWW and commonly known ...&lt;/p&gt;
  &lt;/body&gt;
&lt;/html&gt;
</code></pre>
```

> 还有一些行内代码，我们也应该用 code 标签。



**总结：**

![](/images/HTML语义标签.png)











