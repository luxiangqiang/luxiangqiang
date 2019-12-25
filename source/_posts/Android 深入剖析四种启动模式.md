---
title:  Android 深入剖析四种启动模式
categories: 
- Android 高级进阶
tags: 

- Android
---

![standard](\images\启动模式.jpg)

在 Android 应用开发中，打造良好的用户体验是非常重要的。 Android 系统中的 Activity 可以说一件很赞的设计，它在内存管理上良好的设计，使得多任务管理在Android系统中运行游刃有余。下面小鹿带你全面解析 Android 的四大启动模式

<!-- more -->

# Android 深入剖析四种启动模式

## 一、四大启动模式

> ​	在实际项目中，根据特定的需求需要对每个活动指定恰当的启动模式。**Android** 页面的启动模式分为四种：**standard**、**singleTop**、**singleTask**和**singleInstance**，可以通过 AndroidManifest.xml 中通过给<activity>标签指定 **android:launchMode** 属性来选择启动模式。

### 1、standard

​	 **standard 是活动默认的启动模式。**在不进行设置的情况下，所有的活动都是自动使用这种启动模式。在这里讲启动模式之前，先说一下 **Android** 是利用返回栈来管理活动的。对于基础差的开发者还不理解 **Android** 活动（页面）是怎么进行管理的，就是用到刚刚上边说到的返回栈来管理活动的。

​	我们详细来看一下，根据我自己对任务的理解来详细说一下，**Android** 是使用任务（**Task**）来管理活动的，一个任务就是存放到栈里的活动（页面）的集合，这个栈也成为返回栈（**Back Stack**）,栈是一种先进后出的的数据结构，在默认的情况下，每当启动一个活动，它会在返回栈中入栈，并位于栈的顶端位置，每当用户按Back键或调用finish()方法去毁掉一个活动时，处于栈顶的活动就会出栈，这时，前一个入栈的活动就会处于栈顶位置。也就是说，用户可以看到的活动（页面）就是处于栈顶的活动！（**为了更好的理解我的给大家大体画了个图**）

![standard](\images\standard.png)

**我新建了一个项目，代码如下，我启动三次第一个页面进行测试：** 

``` java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    //控制台打印本页面的存标识
    Log.d("FirstActivity",this.toString());
    setContentView(R.layout.activity_first);
}
//启动本页面
public void Start_Intent(View v){
    Intent intent = new Intent(FirstActivity.this,FirstActivity.class);
    startActivity(intent);
}
```



**控制台打印信息为：** 

![standard](\images\c1.png)

![standard](\images\c2.png)

![standard](\images\c3.png)

从控制台打印信息可以看到，每点击一下按钮都会创建一个 **FirstActivity** 实例，这时，返回栈中也有三个实例，当我们按 **Back** 键退出时需要按三下才能退出程序！**（如图）:** 

![standard](\images\standard1.png)

### 2、singleTop

> 当活动的启动模式指定为 **singleTop**，在启动活动如果发现返回栈栈顶已经是该活动时，就直接去使用它，不会创建新的活动实例。

代码如下：

``` java
<activity android:name=".FirstActivity"
    android:launchMode="singleTop">//启动模式singleTop
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```



``` java
public class FirstActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //控制台打印本页面的存标识
        Log.d("FirstActivity",this.toString());
        setContentView(R.layout.activity_first);
    }
    //启动本页面
    public void Start_Intent(View v){
        Intent intent = new Intent(FirstActivity.this,SecondActivity.class);
        startActivity(intent);
    }
}
```

``` java
public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        Log.d("SecondActivity",this.toString());
    }
    //启动本页面
    public void Start_Intent(View v){
        Intent intent = new Intent(SecondActivity.this,FirstActivity.class);
        startActivity(intent);
    }
}
```

![singleTop](\images\s1.png)

![singleTop](\images\s2.png)

![singleTop](\images\s3.png)

从打印信息可以看出，创建了两个 **FirstActivity** 实例，因为我们在 **FirstActivity** 跳转到 **SecondActivity** 时，位于栈顶的是 **SecondActivity** ,因此会再创建一个新的实例  **FirstActivity** .当用户按下 **Back** 三次才可以退出程序。**下面看示意图**： 

![singleTop](\images\singleTop.png)

### 3、singleTask

​	开发者使用 **singleTask** 可以解决上边 **singleTop** 模式重复创建栈顶活动的问题，为了避免活动不处于栈顶重复创建活动实例的情况，我们可以使用 **singleTask** 的模式。这个模式的特点是什么呢？当每次用户启动一个活动时，系统先检查返回栈的是否存在该活动，如果不存在就创建一个新的实例，如果存在该活动，位于该活动顶部的活动全部出栈，使该活动处于栈顶！代码改一下上方的 **android：launchMode="singleTask"** 可以自己试一下！示意图如下： 

![singleTask](\images\singleTask.png)

### 4、singleInstance

​	singleInstance模式比前边的三种模式要复杂一些，与前边的三个模式不同的是指定为 **singleIntance** 模式的活动会启动一个新的返回栈来管理这个活动，什么时候可以用到这个模式呢？当我们这个程序的活动允许其他程序调用时，要实现这个程序和其他程序共享这个活动的话，前边的三种模式都实现不了，因为每个程序都有自己的返回栈，同一个活动在不同的返回栈中入栈的时候会创建一个新的实例。而 **singleIntance** 模式可以解决这个问题，以为这个模式有一个独立的返回栈来管理这个活动，无论有多少程序来调用这个活动，都共用同一个返回栈。

​	举个例子：我们建立三个页面，分别为**FirstActivity**、**SecondActivity**、**ThirdActivity** 每个页面都要分别用

```
Log.d("FirstActivity","getTaskId()");
Log.d("SecondActivity","getTaskId()");
Log.d("ThirdActivity","getTaskId()");
```

来查看创建返回栈的 **id**,我们将 **singleIntance** 模式指定为 **SecondActivity** 页面，我们实现三个页面的连续跳转，控制台打印出信息为：**121**、**122**、**121**。可以很明白的看出 **SecondActivity** 单独位于一个返回栈中。

​    	FirstActivity*页面跳转到 **SecondActivity** 页面，我来解释一下，因为 **FirstActivity** 和 **ThirdActivity** 位于同一个返回栈中，所以 **ThirdActivity** 位于栈顶出栈，**FirstActivity** 页面位于栈顶，所以 **ThirdActivity** 页面直接跳转到 **FirstActivity** 页面，再按 **Back** 键时，**FirstActivity** 和 **ThirdActivity** 的返回栈为空，就会显示另一个返回栈的活动，当另一个返回栈的活动出栈时，程序才会退出！

**示意图如下：**

![singleInstance](\images\singleIntance.png)

## 二、四大启动模式深入剖析

> **Activity** 的启动模式也是一个难点，原因是形形色色的启动模式和标志位太容易混淆，但是 **Activity** 作为四大组件之首，它的确非常重要，为了满足项目的需求，必须使用 **Activity** 的启动模式。 

### 1、Activity 的 LaunchMode

#### 1.1  启动模式

​	之所以 **Activity** 使用启动模式，因为 **Activity** 的创建是在任务栈中的，当我们启动同一个 **Activity** 时，系统就会创建多个 **Activity** 实例放入任务栈中，当我们按 **back** 键时，任务栈中的实例就会一一出栈。栈我想并不陌生，具有的特点：先进先出。如果我们不允许系统重复创建相同的 Activity ，我们就会用到 **Activity** 的启动模式进行设置。**Activity** 的启动模式分为四种 **standard、singleTop、singleTask**和 **singleInstance,**之前的那篇文章也有相关介绍，下面就简单提一下。

##### （1）standard 标准模式 :

​	这是系统默认的启动模式，每次启动一个 **Activity** 都会创建一个新的实例，不管这个实例是否存在。如果 **A** 启动了 **B**，**B** 的活动就会进入到 **A** 的任务栈中。

##### （2）singleTop 栈顶复用模式 :

​	在这种启动模式下，新的 Activity 已经位于栈顶，如果再次启动该 **Activity** ，此 **Activity** 不会被重新创建。同时系统的 **onNewIntent** 方法被回调，通过此方法的参数我们可以取出当前的请求信息。当然 **Activity** 的 **onCreate**、**onStart** 和 **onResume** 方法不会重新被调用。如果该 **Activity** 没有位于栈顶，该活动就会重新被创建。

##### （3）singleTask 栈内复用模式 :

​	这是一种单例模式，在这种模式下，只要栈中存在该实例，该实例不会被重新创建。比如：我们想要创建一个实例 **A**，系统就会先判断任务栈中是否存在和 **A** 同样的实例。如果实例存在任务栈中，系统就会把 A 调用到栈顶并调用它的 **onNewIntent** 方法，同时 **A** 以上的 **Activity** 实例都会被移除出栈直到 A 位于栈顶位置；如果实例不存在，系统就会创新创建一个新的实例 **A** 并将其压入栈顶。

##### （3）singleInstance 单实例模式 :

​	我通常把这种模式的 **Activity** 称为 **singleTask** 模式的加强版，除了具有 **singleTask** 模式具有的特点外，以 **singleInstance** 启动的 **Activity** 实例单独存在一个任务栈中，后续的请求不会创建新的实例。

#### 1.2 任务栈

什么是任务栈？各个 **Activity** 是怎么样分配到各个任务栈的？以下情况都是在 **singleTask** 模式情况下来说的。 

> 从一个参数说起，**TaskAffinity** ，翻译为任务的相关性，这个参数标识了一个 **Activity** 所需要得任务栈的名字，默认情况下，**Activity** 所需要的任务栈的名字为应用的包名。任务栈分为前台任务栈和后台任务栈，后台任务栈中的 Activity 处于暂停状态，用户可以将后台任务栈切换到前台。                                ——任务栈 

#### 1.3 设置启动模式的两种方式

- **第一种方式 :** 通过 AndroidMenifest 配置文件设置启动模式

  ``` java
  <activity
        android:name=".MonitoringActivitys.MonitorActivity"
         android:label="@string/title_activity_monitor"
         android:theme="@style/AppTheme.NoActionBar"
         android:launchMode="singleTask">
         <intent-filter>
             <action android:name="android.intent.action.MAIN" />
             <category android:name="android.intent.category.LAUNCHER" />
         </intent-filter>
   </activity>
  ```

- **第二种方式 :** 通过 Intent 中设置标志位来设置启动模式
  ``` java
  Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
  intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
  startActivity(intent);
  ```

  **区别：**

  **①** 第二种优先级要高于第一种 

  **②** 第一种无法给 **Activity** 设定 **FLAG_ACTIVITY_CLEAR_TOP** 标识，第二种无法为 **Activity** 指定 **singleInstance** 模式。 

  

  