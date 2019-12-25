---
title: 前端面试之【CSS3的新特性】
categories: 
- 前端
- 面试
tags: 
- 前端
- 面试
---

CSS3 的有哪些新特性？

<!--more-->



### 一、CSS3新增选择器

- `p:first-of-type` |  `p:last-of-type` ` 选择所有父元素该元素出现的第一个(最后一个)元素。
- `p:nth-child(2)`  选择父元素的第二个元素。
- `p:first-child`  | `p:last-child`  选择父元素的第一个元素或最后一个元素。
- `p:only-of-type` 选择父元素唯一没有兄弟节点的元素。
- `:enabled`  `:disabled` 选择表单禁用状态的输入框。
- `:checked`  选择单选按钮或复选框被选择状态的。



### 二、文本

- `text-shadow:参数1 参数2 参数3 参数4`   向右偏移  向下偏移  渐变像素  渐变颜色；
- `white-space: nowrap;` 禁止文字换行；
- `text-overflow: ellipsis ` 文字溢出部分显示省略号（必须隐藏元素overflow:hidden）；
  - `clip: `文字溢出部分剪切掉。
- `word-wrap:`  对长的不可分割的单词进行分割并换行到下一行



### 三、边框

- `border-radius: `圆角边框

- `border-shaow: `盒阴影

- `border-shadow:` 右偏移 下偏移  模糊程度 阴影颜色

- `border-images-*: `

  - `source` 图片路径 
  - `slice` 向内偏移 
  - `width` 边框宽度 
  - `outset`边框区域超出边框的量 
  - `repeat :` `repeat/round/stretch` 是否平铺/铺满/拉伸

  



### 四、背景

- `background-image:`背景图片
- `background-size：`指定背景图片的大小 cover : 缩放不变 /contain：保持最大大小
- `background-origin：`定位背景图片的位置 `content-box/border-box/padding-box `
- `background-clip：`规定背景的绘制区域



### 五、渐变

> 渐变分为线性渐变（Linear-gradient）和径向渐变(Radial-gradient)

#### 1、线性渐变

- `background:linear-gradient (起始位置，起始颜色，终止颜色); `
  - top bottom left right 以及对角线组合;
  - 自定义角度（30deg）— 顺时针;
  - 颜色可多个组合;
  - 透明度渐变(rgba CSS3新属性);
- `repeating-linear-gradient()` 函数用于重复线性渐变;

#### 2、径向渐变

- `background: radial-gradient(center, shape size, start-color, ..., last-color);`
  - center 渐变中心
  - 指定渐变颜色大小 size （不均匀分布）
  - 第一参数：设置形状（circle、ellipse椭圆）
  - 也可以设置成尺寸

- `repeating-radial-gradient()` 函数用于重复线性渐变;



### 六、过渡



### 七、动画、旋转