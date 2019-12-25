---
title: 前端 JS 基础篇之【浏览器中的高度】
categories:
- 前端
tags:
- 前端
---

浏览器中的高度由内而外分为页面、窗口、屏幕！

<!--more-->



## 浏览器相关高度

### 一、页面尺寸

![é¡µé¢å°ºå¯¸](https://img-blog.csdn.net/20161228000813185?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3M2NTEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 1、Body 的总高度/总宽度

> 页面的可滚动的高度也算。

- `body.offsetHeight`
- `body.offsetWidth`



#### 2、Body 的可见区域高度/宽度

> 只展现出的可视化页面高度/宽度。

- `body.clientHeight`
- `body.clientWidth`



#### 3、滚动条的高度

```
window.Height - body.clientHeight = 滚动条的高度
```



### 二、浏览器尺寸

![è·åæµè§å¨å°ºå¯¸](https://img-blog.csdn.net/20161228000146700?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3M2NTEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 1、浏览器的高度

> 整个浏览器（内容+工具栏+滚动条）的高度/宽度。

- `window.outerHeight`
- `window.outerWidth`



#### 2、页面可用高度

> 除去工具栏只有（显示内容区域内容+滚动条）。

- `window.innerHeight`
- `window.innerWidth`

可求出工具栏的高度：

```
window.outerHeight - window.innerHeight
```



### 三、屏幕的尺寸

![å±å¹å°ºå¯¸çè·å](https://img-blog.csdn.net/20161227235703448?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3M2NTEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 1、屏幕高度

> 整个屏幕的高度（内容+任务栏）。

- `screen.Height`
- `screen.Width`



#### 2、屏幕可用高度

> 除去任务栏的屏幕可用高度。

- `screen.availHeight`
- `screen.availWidth`



