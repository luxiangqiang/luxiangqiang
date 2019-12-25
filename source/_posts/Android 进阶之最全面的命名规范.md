---
title: Android  进阶之最全面的命名规范
categories: 
- Android 初级进阶
tags: 
- Android 
---

![](\images\Android命名规范.png)

今天分享一下 Android 的命名规范，要养成一个良好的规范习惯对以后工作很重要！ 

<!--more-->



# Android 进阶之最全面的命名规范

## 目录

![](\images\Android 命名规范目录.png)



## 1. 为什么规范 Android 代码命名？

- 增强代码的可读性
- 增强代码的可维护性

正由于上述两个作用，从而使得 **开发效率 & 维护效率** 得到大幅度的提高。



## 2. Android需要命名的代码（对象）有哪些？

![](\images\Android命名规范1.png)



## 3. 具体命名规范

下面，我将对 `Android` 代码对象中的命名规范进行详细讲解

> 注：由于  `Android`主要用`Java`实现，所以Android规范会涵盖部分Java规范

### 3.1 包

- 基础规则：小写、单词间连续无间隔、反域名法（分为4级，具体如下图）

  ![](\images\Android命名规范2.png)

  

- **第4级包名会随着功能的不同而不同**。下面我列举出一些常见 & 需要规范的4级功能包名

  ![](\images\Android命名规范3.png)

  

### 3.2 类

- 基础规则 
  1. 类型 = 名词 / 名词短语；
  2. 形式 = 驼峰形式中的 **大骆驼拼写法**（`UpperCamelCase`）

> 即名称中的每个词的首字母都大写，如 `AndroidStudio`

- 在具体命名类时，会根据 **该类的类型不同而附加额外的命名规则**。具体如下图

  ![](\images\Android命名规范4.png)

### 3.3 变量

- 基础规则
  1. 类型 = 名词 / 名词短语；
  2. 形式 = 驼峰形式中的 **小骆驼拼写法**（`LowerCamelCase`）

> 即名称中的第1个词的首字母小写，后面每个词的首字母大写，如`androidStudioTool`

- 在具体命名变量时，会根据**该变量的类型不同而 附加额外的命名规则**。具体如下图 

![](\images\Android命名规范5.png)

### 3.4 方法

- 基础规则 
  1. 类型 = 动词 / 动词短语；
  2. 形式 = 驼峰形式中的 **小骆驼拼写法**（`LowerCamelCase`）

> 即名称中的第1个词的首字母小写，后面每个词的首字母大写，如`androidStudioTool`

- 在具体命名 方法名时，会根据 **该方法名的作用不同而 附加额外的命名规则**。具体如下图

  ![](\images\Android命名规范6.png)

  

### 3.5参数名

- 基础规则：驼峰形式中的 **小骆驼拼写法**（`LowerCamelCase`）

  > 即名称中的第1个词的首字母小写，后面每个词的首字母大写，如`androidStudioTool`

- 附加命名规则：功能名，如`userName` 



### 3.6 资源

- Android的资源包括：

  ![](\images\Android命名规范7.png)

  ![](\images\Android命名规范8.png)

  

  下面，我将对每种`Android`资源的命名规则进行详细讲解

#### 3.6.1 布局文件资源

![](\images\Android命名规范9.png)



#### 3.6.2 图片资源

![](\images\Android命名规范10.png)



#### 3.6.3 参数值资源

![](\images\Android命名规范11.png)



#### 3.6.4 动画资源

![](\images\Android命名规范12.png)



### 3.7 额外

![](\images\Android命名规范13.png)

至此，关于`Android`的代码命名规范讲解完毕



## 4. 附录：常见使用单词缩写表

- 使用单词缩写的原则：只使用约定俗成的单词缩写

> 严禁自由缩写单词

- 具体如下图

![](\images\Android命名规范14.png)























