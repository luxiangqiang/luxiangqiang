---
title: 前端 CSS3 之基础篇【转换】
categories:
- 前端
- CSS3
tags:
- 前端
- CSS3
---



小鹿带你走进 CSS3 转换，好玩又是重点。

<!--more-->

### 一、Treansform

> CSS3 变形属性。
>
> 让元素在一个坐标系中变形。这个属性包含一系列的变形函数，可以移动、旋转和缩放元素。



###### ▉ 语法

```
transform:none | <tranform-function>
```



###### ▉ 兼容性

```
IE12+、ForeFox16+、Chrome36+、Safari16+、Opera23+
```



### 二、2D 转换

- rotate() 旋转
- translate() 平移
- scale() 缩放
- skew() 扭曲和斜切
- matrix() 矩阵或混合



#### 1、旋转 rotate

> 通过指定的角度参数对原元素指定一个 2D rotation （2D 旋转）。



###### ▉ 语法

```
transform: rotate(<angle>);
```



###### ▉ 参数说明

angle 指旋转角度，正数表示顺时针旋转，负数表示逆时针旋转。



#### 2、移动 translate 

> 根据左（X轴）和顶部（Y轴）位置给定的参数，从当前元素位置移动。



###### ▉ 语法

```
transform: translate ();
```



###### ▉ 三种情况

- translateX(x) 仅仅水平方向移动（X 轴移动）；
- translateY(Y) 仅垂直方向移动（Y 轴移动）;
- translate(x,y) 水平方向和垂直方向同时移动（也就是 X 轴和 Y 轴同时移动）。



#### 3、scale 缩放

> 2D 缩放。



###### ▉ 三种缩放

- scaleX()  水平方向缩放
- scaleY()  垂直方向缩放
- scale()  x y 同时缩放



#### 4、扭曲 skew 

> 斜切扭曲。



###### ▉ 三种扭曲 

- skewX（）水平扭曲（正值，逆时针）
- skewY（）垂直扭曲（正值，顺时针）
- skew（）水平垂直同时扭曲



#### 5、矩阵 matrix（难点）

> 矩阵分为 matrix 和 matrix3d 矩阵，分别为 3 * 3 和 4 * 4 的矩阵。





















