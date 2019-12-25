---
title: 前端 CSS3 之基础篇【图片切换】
categories:
- 前端
- CSS3
tags:
- 前端
- CSS3
---

CSS3 实现图片多样式切换！

<!--more-->



#### 一、不同格式的字体对浏览器的兼容性不同

| Browser                                                      | @font-face | TrueType | WOFF  | EOT  | SVG  | SVGZ |
| ------------------------------------------------------------ | ---------- | -------- | ----- | ---- | ---- | ---- |
| ![img](https://pic002.cnblogs.com/images/2011/36987/2011020918254839.png)IE | 4+         | 9+       | 9+    | 4+   |      |      |
| ![img](https://pic002.cnblogs.com/images/2011/36987/2011020918264049.png)火狐 | 3.5+       | 3.5+     | 3.6+  |      |      |      |
| ![img](https://pic002.cnblogs.com/images/2011/36987/2011020918271360.png)谷歌 | 4+         | 4+       | 6+    |      | 4+   | 6+   |
| ![img](https://pic002.cnblogs.com/images/2011/36987/2011020918265934.png)苹果 | 3.1+       | 3.1+     | 6+    |      | 3.1+ | 3.1+ |
| ![img](https://pic002.cnblogs.com/images/2011/36987/2011020918262177.png)opera | 10+        | 10+      | 11.1+ |      | 10+  | 10+  |

#### 二、内联元素的自动转换

> 当给一个**内联**元素加以下属性就会转换为**块元素**。
>
> 1、float;
>
> 2、position;
>
> 3、absolute;

