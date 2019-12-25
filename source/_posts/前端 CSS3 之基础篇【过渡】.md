---
title: 前端 CSS3 之基础篇【过渡】
categories:
- 前端
- CSS3
tags:
- 前端
- CSS3
---

CSS 过渡会用到什么地方呢？

<!--more-->



## 一、CSS3 过渡

> 1、允许 css 的属性值在一定的时间区间内平滑地过渡。
>
> 2、在鼠标点击、获得焦点、被点击或对元素任何改变中触发，并圆滑地以动画效果改变 css 的属性值。



###### ▉ 兼容性

> **IE 10+**、FireFox16+、Chrome26+、Safari6.1+ Opera 12.1 +



## 二、CSS3 transition 属性(4个)

#### 1、transition-property 属性

> 检索或设置对象中的参与过渡的属性。



###### ▉ 语法

```css
transition-property:none|all|property
```

参数说明：

- none : 没有属性改变
- all : 所有属性改变，默认值
- property: 元素的属性名称



#### 2、transition-property 属性

> 检索或设置对象过渡的持续时间。



###### ▉ 语法

```css
transition - duration: time;
```

参数说明：

- 规定完成过渡效果需要花费的时间（以秒或毫秒计）
- 默认为 0



#### 3、transition-timing-function 属性

> 检索或设置独享中的过渡的当动画类型。



###### ▉ 语法

```css
transition-timing-function：ease|linear|ease-in|ease-out|ease-in-out|step-start|step-end
```

参数说明：

- linear：匀速过渡
- ease：平滑过渡（慢—快—慢）
- ease - in：由慢到快
- ease - out: 由快到慢
- ease - in - out：由慢到快再到慢（最人性化）
- step-start：等同于 steps(1,start)
- step-start：等同于 steps(1,end)



#### 3、transition-delay 属性

> 检索或设置对象延迟过渡的时间。



###### ▉ 语法

```css
transition-delay：time;
```



#### 4、transition 复合属性

> 复合属性



###### ▉ 语法

```css
//属性 过渡时间 类型 延迟
transition:property duration timing-fnction delay;
```

