---
title: Android 深入剖析生命周期
categories: 
- Android 高级进阶
tags: 
- Android

---

![liftCircle](/images/生命周期.jpg)

 Activity 生命周期是研究Android开发最基础的知识点之一，熟练掌握生命周期的特性可以在实际开发中避免踩坑。比如，一些UI的初始化和回收操作、框架库的注册于反注册（摧毁）、线程的启动和停止等。 

<!-- more -->

#  Android 的生命周期深入剖析

## 一、正常情况下生命周期

![liftCircle](/images/shengmingzhouqi.png)

- **onCreate** : **表示页面（Activity）的创建**。（生命周期第一个阶段）功能：完成初始化工作，如：加载页面布局资源、初始化数据。
- **onStart** : **表示页面（Activity）正在被启动**，即将开始。功能：页面为可见状态，但是无法与用户交互。
- **onResume** : **表示页面（Activity）出现在前台**。功能：与 **onStart** 相比，**onStart** 处于后台，**OnResume** 才显示到前台。
- **onPause** : **表示页面（Activity）正在停止**。功能：页面处于后台，正常情况下，**onStop** 紧接着执行。此时会做一些数据存储、停止动画不太耗时的工作。**onPause** 执行完新的页面（Activity）的 **onResume** 才会执行。
- **onStop** : **表示页面（Activity）即将停止**。功能：页面为不可见状态，做稍微轻量级的不太耗时的回收工作。
- **onDestroy** : **表示页面（Activity）即将销毁**。（生命周期最后一个阶段）功能：回收工作和资源的释放。
- **onRestart** : **表示页面（Activity）重新启动**。功能：页面从不可见状态转化为可见状态时会调用此方法。如：**Home** 键切换页面（打开新的 **Activity**），然后回到页面过程中。 

### 问题：

**1、onStart 和 onResume、onPause和onStop从描述上来看差不多，对我们来说有什么实质上的不同？**

**答**：**onStart** 和 **onStop** 是从 **Activity** 是否可见这个角度来回调的，而 **onResume** 和 **onPause** 是从是否位于前台来回调的。 

**2、假设当前 Activity 为 A。如果用户打开一个新的 Activity B，那么 B 的 onResume 和 A 的 onPause 哪个先执行？** 

**答**：根据 **Android** 的基本运行机制，不能再 **onPause** 中执行重量级的操作，因为必须 **onPause** 执行完成以后新 **Activity** 才能 **onResume**。**onPause** 和 **onResume**  都不能执行耗时的操作，尤其是 **onPause**，这就意味着我们应该在 **onStop** 中做操作。从而使新的 **Activity** 显示出来并切换到前台。 

## 二、异常情况下生命周期

### 1、情况一：资源相关的系统配置发生改变导致 Activity 被杀死并重新创建

![catchLiftCirecle](/images/yichang.png)

比如当前Activity处于竖屏状态，如果突然旋转屏幕，由于系统配置发生改变，在默认情况下，Activity 就会被销毁并且重新创建。 

**（1）异常生命周期调用过程：** 

​	① **Activity** 会被销毁，其中 **onPause**、**onStop**、**onDestory**均会被调用，同时由于 **Activity** 是在异常情况下终止的

​	② 系统会调用 **onSavaInstanceState** 来保存当前的 **Activity** 的状态。这个方法的调用时机是在 **onStop** 之前，它和 **onPause** 没有既定的时序关系，它既可能在 onPause 之前或者之后调用。这种情况只会出现在 **Activity** 被异常终止的条件下。

​	③ 当 **Activity** 被重新创建后，系统会调用 **onRestoreInstanceState**,并且把 **Activity** 销毁时的 **onSaveInstanceState** 方法所保存的 **Bundle** 对象作为参数同时传递给 **onRestoreInstanceState**  和 **onCreate** 方法。因此我们可以通过判断 **onRestoreInstanceState**  和 **onCreate** 方法是否被重建。

​	④ 如果被重建了，我们会取出之前保存的数据并恢复，从时序上来说，**onRestoreInstanceState**  的调用在 **onStart** 之后。

**（2）需要注意的两点：**

**★ 我们销毁 Activity 重新创建获取数据状态时，有两种方式，接收位置可以选择 onRestoreInstanceState（官方建议使用）或者 onCreate  两者的区别。**

​	① **onRestoreInstanceState**  一旦被调用，其参数 **Bundle** **saveInstanceState** 一定是有值的，我们不用额外的判断是否为空。

​	② **onCreate** 正常启动的话，其参数 **Bundle** **saveInstanceState** 为 **null**,所以必须进行额外的判断。

​	③ 系统只有在 **Activity** 异常终止的时候才会调用 **onSaveInstanceState**  和 **onRestoreInstanceState**  来存储和恢复数据，其他情况不会出发这个过程。

### 2、情况二：资源内存不足导致低优先级的 Activity 被杀死

**（1）先描述一下 Activity 的优先级情况，Activity 的优先级从高到底，可分为一下三种：**

​	① 前台 **Activity** —— 正在和用户交互的 **Activity** ，优先级最高。

​	② 可见但非前台的 **Activity** —— 比如 **Activity** 中弹出了一个对话框，导致 **Activity** 可见但是位于后台和无法与用户直接交互。

​	③ 后台 Activity —— 已经被暂停的 **Activity** ，比如执行了 **onStop** ，优先级最低。

**（2）资源内存不足时的过程分析：** 

​	① 当系统资源不足时，系统会按照上述优先级去杀死目标的 **Activity** 所在的进程，并在后续通过 **onSaveInstanceState** 和 **onRestoreInstanceState** 来存储和恢复数据。如果一些后台的进程脱离了四大组件而独立运行，那么这个进程很快就被杀死。我们常常将后台工作放到 **Service** 中保持进程具有一定的优先级。 

**（3）问题：当系统发生改变时，我们不想让 Activity 发生改变，比如，当我们旋转屏幕时，不想重新创建新的 Activity ，我们会怎么操作？** 

​	如果我们没有在 **configChanges** 属性中指定选项的话，当系统配置发生改变的话 **Activity** 就会被重新创建。我们常用到的三个属性：

​	 **① locale : ** 设备的本地位置发生了改变，一般指切换了系统语言。

​	 **② keyboardHidden :** 键盘可访问性发生了改变，比如用户调用了键盘。

​	 **③ orientation ：** 屏幕方向发生了改变，这个是最常用到的，比如旋转的手机屏幕，一般与 **screenSize** 属性值配合使用。

​	这样，**Activity** 不会被创建，**onSaveInstanceState** 和 **onRestoreInstanceState**  方法不会被调用，取而代之，系统调用了 **onConfigurationChanged** 方法，这个时候我们可以做一些特殊的处理了。

