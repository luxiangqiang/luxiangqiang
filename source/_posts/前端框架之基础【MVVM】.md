---
title: 前端面试之道之框架【MVC/MVP/MVVM】
categories: 
- 前端
- 面试
tags: 
- 前端
- 面试
---

什么是MVC/MVP/MVVM？

<!--more-->



## MVC

> 使用了两种模式，观察者模式和策略模式。

- `Model` 和 `View` 之间使用的是**观察者模式**;
- `View` 和 `Controller` 之间采用的是**策略模式**;
- `Controller`  是 `View` 和 `Model` 之间的连接的枢纽。



#### 一、Model 模型层

> 模型层主要封装了**应用程序以及业务逻辑**所使用到数据和处理数据的方法。Model 和 View 之间使用的是**观察者模式**，View 事先在 Model 上进行注册，一旦 Model 的数据发生改变，就会做出相应的改变。



#### 二、View 视图层

> 视图层主要是负责数据的展示。View 和 Controller 之间采用的是**策略模式**，不同的 View 引用不同的 Controller 的实例来实现了特定的**响应策略**（比如点击事件）。



#### 三、Controller 控制层

> 控制层主要用来连接 Model 和 View 。**使用控制层来对用户界面的用户输入响应**，连接模型层，用于控制对数据的改变，所有的业务逻辑主要集中在控制层。



## MVP

> MVP 主要加大了 Model 和 View 解耦， View 不能直接获取 Model 层的数据，而是通过 Presenter 提供给 View 接口，负责将 View 和 Model  的数据进行手动同步。



## MVVM

> MVVM 主要用框架封装了 MVP 中的 Presenter 的手动操作，主要实现了自动化的数据绑定，只要告诉 View 显示的数据是 Model 的哪一部分就可以了。



### 一、MVVM 出现的原因？

> 1、由于前端开发混合了 HTML 、CSS、Javacript 众多页面，所以代码的组织和维护变的更加复杂，所以就出现了 MVVM 模式的原因。
>
> 2、MVVM 最早是微软提出来的，其实借鉴了桌面应用程序的 MVC 设计思想。



#### 二、什么是 MVVM ？

> MVVM 就是 Model-View-ViewMoudel 的缩写。

Model 就用纯 JavaScript 对象表示，View 负责显示，两者做到了极大限度的分离。把 Model 和 View 关联起来就是 ViewModel。ViewModel 主要负责把 Model 的数据同步到 View 显示，还负责把 View 修改的数据同步回 Model。



#### 1、Model

> Model 视图层，只关注视图本身，可以理解成 json 对象。



#### 2、View

> MVVM 中的 View 通过使用模板语法声明式的渲染 DOM，当 ViewModel 对 Model 进行更新的时候，通过数据绑定更新到 View。



#### 3、ViewModel

> 与 MVP 不同的是，没有了 View 为 Presente 提供的接口，之前由 Presenter 负责的 View 和 Model之间的数据同步交给了 ViewModel 中的**数据绑定**进行处理，当 Model 发生变化，ViewModel 就会自动更新；ViewModel 变化，Model 也会更新。









