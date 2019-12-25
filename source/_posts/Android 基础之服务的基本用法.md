---
title: Android 基础之服务的基本用法
categories: 
- Andoid
tags: 
- Android 基础
---

![](\images\Android服务.png)

服务是Android 四大组件之一，是非常重要的知识点，下面是 Android 服务的基本用法入门基础讲解。

<!--more-->

# Android 四大组件之一服务（service）

## 定义一个服务

**1、新建服务new —> Service — > .java 文件（定义一个服务就必须在里边实现相应的操作）**

```java
//服务创建的时候会调用
@Override
    public void onCreate() {
        super.onCreate();
    }
//每次服务启动的时候调用(一旦启动服务，在其方法执行的工作)
 @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return super.onStartCommand(intent, flags, startId);
    }
//会在服务销毁的时候调用
   @Override
    public void onDestroy() {
        super.onDestroy();
    }
```

以上是服务最常用到的三个方法。

**2、使用服务之前，需要在 AndroidManifest.xml 中注册服务**

```java
 <!--服务注册-->
 <service
    android:name=".MyService" //服务的类名
    android:enabled="true"    //是否允许除当前程序之外的其他程序访问
    android:exported="true">  //是否启动这个服务
 </service>
```



## 启动和停止服务

### 启动服务

```java
 Intent startintent = new Intent(this,MyService.class);
 startService(startintent);
```

### 停止服务

```java
Intent stopIntent = new Intent(this,MyService.class);
stopService(stopIntent);
```



### 活动和服务进行通信

> 上面我们学习到了服务的启动和停止，我们也知道服务实在活动中启动的，但是活动怎么才能知道服务去干吗了呢？怎么样用活动去指挥服务去完成什么样的任务呢？



### 实例

**1**、**在 MyService.java 类中创建一个管理下载的 DownloadBinder 类**

```java
// 用Binder对象来对下载功能进行管理
class DownloadBinder extends Binder {

    public void startDownload(){
        Log.d("MyService","开始下载...");
    }

    public void getProgress(){
        Log.d("获取下载进度","获取到了下载进度");
        return;
    }
}
```

我们在布局中创建两个按钮分别（绑定服务）和（取消绑定服务）。



**2、创建 ServiceConnection 的匿名类，里边重写了两个方法，分别会在活动与服务成功绑定以及活动与服务的连接断开的时候调用，绑定成功时就会调用相应的方法**

```java
 private MyService.DownloadBinder downloadBinder;

//活动连接服务的对象
private ServiceConnection connection = new ServiceConnection() {
    /**
     * 功能：绑定成功
     * @param componentName
     * @param iBinder
     */
    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        downloadBinder = (MyService.DownloadBinder) iBinder;
        //开始下载
        downloadBinder.startDownload();
        //获取下载进度
        downloadBinder.getProgress();
    }

    /**
     * 功能：服务和活动断开时调用
     * @param componentName
     */
    @Override
    public void onServiceDisconnected(ComponentName componentName) {

    }
};
```



**3、绑定服务**

```java
Intent bindIntent = new Intent(this,MyService.class);
/**
 * 功能：将 MainActivity与 MyService进行绑定
 * 参数一：bindIntent 启动服务Intent对象
 * 参数二：connection 活动和服务的连接
 * 参数三：BIND_AUTO_CREATE 活动和服务绑定成功后自动创建服务（使MyServiced的onCreate执行）
 */
bindService(bindIntent,connection,BIND_AUTO_CREATE);//绑定服务
```



**4**、**解除服务**

```java
unbindService(connection);
break;
```



## 服务的生命周期

1、项目的任何位置调用了 Context 的 startService() 方法，服务就会回调 onStartCommand（）方法启动服务。

2、如果这个服务之前没有创建过，onCreate（）方法就会比 onStartCommand（）先执行。服务启动一直保持运行状态，直到 stopService（）或 stopSelf（）方法被调用。

3、每调用一次 startService（）方法，onStartCommand（）方法就会执行一次。

4、调用 Context 的 bindService（）来获取一个服务的持久连接，之后回调 onBind 方法。

5、如果这个服务之前没有创建过，onCreate（）方法就会比 onBind（）方法先执行。





