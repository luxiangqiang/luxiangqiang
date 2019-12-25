---
title: 前端 HTML5 基础篇之【新增标签】
categories:
- 前端
- HTML5
tags:
- 前端
- HTML5
---

第一节：HTML 5 有哪些新增标签，各有什么作用呢？

<!--more-->

# HTML5

## 一、新增标签

#### 1、结构标签(10个)

> 块状元素——有意义的div。（10个可根据网页记忆）

■ `article `：标记一篇文章。

■ `header` ：标记一个页面的头部或一个区域的头部。

■ `nav`：标记定义导航链接。

■ `section`：标记定义一个区域。

■ `aside`：标记定义页面内容部分的侧边栏。

■ `hgroup`：标记定义文件中的一个区块的相关信息。

■ `figure`：标记定义一组媒体内容以及它们的标题。

■ `figcaption`：标记定义 figure元素的标题。

■ `footer`：标记定义一个页面或一个区域的底部。

■ `dialog`：标记定义一个对话框类似微信。



#### 2、多媒体标签（5个）

■ `<video src=" " autoplay=" " loop=" " controls=" ">` ：标记定义一个视频。

```html
//对于不是 mp4 格式的视频，使用 source 来解决
<video  src=" " autoplay="autoplay" width=" " height=" ">
	//需要进行转码
	<source src=" " type="video/mp4" />
</video >
```

■ `<audio>`：标记定义音频内容。

■ `<source>`：标记定义媒体资源。

```
<video src=" " autoplay=" " loop=" " controls=" ">您的浏览器不支持！</video>
```

```
//对于不是 mp3 格式的音频，使用 source 来解决
<audio autoplay=" ">
	//需要进行转码
	<source src=" " type="audio/mpeg" />
</audio>
```



■ `<canvas>`： 画布。

■ `<embed>`：标记定义外部的可交互的内容或插件，比如 flash。



#### 3、Web 应用标签（5个)

###### ▉ 状态标签

■  `<meter>`：状态标签（实时状态显示：气压、气温）

> 浏览器兼容：Chrome、Opera。

```html
<meter value="380" min="20" max="380" low="200" high="240" optimum="220"></meter>
<meter value="0.75">75%</meter>
```



■  `<progress>`：状态标签（任务过程：安装、加载)

> 浏览器兼容：Chrome、Firefox、Opera

```html
<progress value="30" max="100"> </progress>
<progress max="100"> 
```



###### ▉ 列表标签

■ `<datalist>`：为 input 添加下拉列表。

> 浏览器的兼容性：Firefox、Opera。

```html
<input placeholder="请选择您喜欢的手机品牌" list="phoneList"/>
<datalist id="phoneList">
    <option value="iphone"></option>
    <option value="sumsung"></option>
    <option value="HUawei"></option>
    <option value="HTC"></option>
    <option value="Meizu"></option>
</datalist>
```



■ `<details>`：隐藏、显示详细内容。

> 浏览器兼容 ：Chrome。

```html
<details>
    <summary>问候</summary>
    <p>你好你好，你好，你好，你好</p>
    <p>你好你好，你好，你好，你好</p>
    <p>你好你好，你好，你好，你好</p>
    <p>你好你好，你好，你好，你好</p>
    <p>你好你好，你好，你好，你好</p>
    <p>你好你好，你好，你好，你好</p>
    <p>你好你好，你好，你好，你好</p>
    <p>你好你好，你好，你好，你好</p>
</details>
```



###### ▉ Menu 标签

■ `<menu>`：工具栏菜单

> 大部分浏览器不支持。



#### 4、其他标签

■ `<mark>`：标记定义有标记的文本（黄色选中状态）。

■ `<output>`：标记定义一些输出类型，计算表单结果配合 oninput 事件。

■ `<keygen>`：标记定义表单里一个生成的键值（加密信息传送）。

■ `<time>`：标记定义一个日期/时间，目前所有的主流浏览器都不支持。





## 二、删除的标签

### 1、纯表现的元素

> **html：结构层；CSS：表现层；**
>
> Basefont、Big、Center、font、s、strike、tt、u。



#### 2、对可用性产生负面影响的元素

> 框架级元素。
>
> 1、frameset 会去掉 body ，破坏 html 的结构。
>
> frame、frameset、noframes



#### 3、产生混淆的元素

> acronym、applet、isindex、dir。
>



## 三、重定义标签

■ `<b>` :代表内联文本，通常是粗体，没有传递表示重要的意思。

■ `<i>`：代表内联文本，通常是斜体，没有传递表示重要的意思。

■ `<dd>`：描述，可以同 details 与 figure 一同使用，定义包含文本，dialog 也可用。

■ `<dt>`：标题，可以同 details 与 figure 一同使用，汇总细节，dialog 也可用。

■ `<hr>`：表示主题结束，而不是水平线，虽然显示相同。

■ `<small>`：表示小字体，例如打印注释或者法律条款。

■ `<strong>`：表示重要性而不是强调符号。





