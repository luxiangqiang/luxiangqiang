---
title:  Android 深入剖析消息机制
categories: 
- Android 高级进阶

tags: 

- Handler 
- Android

---

![siweidaotu](/images/消息机制.jpg)

Android 的消息机制主要是指 Handler 的运行机制，对于大家来说Handler已经是轻车熟路了，可是真的掌握了Handler？本文主要通过几个问题围绕着Handler展开深入并拓展的了解。 小鹿带你全面深入了解 Android 消息机制。

<!-- more -->

# Android 的消息机制深入剖析

Android 的消息机制主要是指 Handlerr 的运行需要底层的 MessageQueue 和 Looper 的支撑。

**（1）MessageQueue 的中文翻译是消息队列。**以队列的形式对外提供插入和删除工作。虽然叫做消息队列，但是内部存储结构并不是真正的队列，而是以单链表的数据结构来存储消息列表。

**（2）Looper 的中文翻译为循环，我们叫它消息循环。**由于 MessageQueue 只是一个存储单元，不会去处理消息。而 Looper 确弥补了这个功能，Looper 会以无限无限循环的形式去查找是否有新的消息，有的话就去处理消息，否则就一直等待。

### 学习思维导图：

![siweidaotu](/images/siweidaotu.jpg)


## 一、Android 消息机制概述

> Android 消息机制主要是指 Handler 的运行机制以及 Handler 所附带的 MessageQueue 和 Looper
> 的工作过程。Handler 的主要作用就是将一个任务切换到某个指定的线程中去执行。
### 1、概述
**（1）、思考：**为什么 Android 要提供这个功能呢？
**答：**因为 Android 规定访问 UI 线程只能在主线程中进行的，如果在子线程中访问 UI ，那么程序就会抛出异常。

**（2）、源码：**经过查看看源码的 checkThread（）方法对更新 UI 是否在主线程中更新，进行抛出异常信息提示开发者（相信在开发中都遇到过这个种情况）。

**（3)、过程：**由于以上限制，这就要求开发者必须在主线程中更新 UI，但是 Android 又建议不要在主线程中进行过于耗时的工作，否则会产生应用程序无响应 ANR。考虑到这种情况，当我们在服务器拉去一些信息并显示到 UI 上时，拉去工作我们将在子线程中进行，拉取完毕之后不能再子线程中直接更新 UI ，没有 Handler ，那我们的确没有办法将访问 UI 的工作切换到主线程去执行。因此，系统之所以给我们提供 Handler，**主要原因是为了解决在子线程中无法访问 UI 的矛盾。**

**（4)、问题：**

**① 为什么不能再子线程中更新 UI？**
**答 ： **因为 Android 的 UI 控件不是线性安全的。如果在多线程中并发的访问可能会导致 UI 控件处于不可预期的状态。

**② 为什么不对 UI 控件的访问加上锁机制呢？**
**答 ：**首先，加上锁机制会让访问 UI 变的复杂，其次锁机制会降低 UI 的访问效率，因为锁机制会阻塞某些线程的执行。

最简单最高效的就是采用单线程模型来处理 UI 操作，只需通过 Handlerr 切换一下 UI 访问的执行线程即可。

### 2、Handler 的工作原理

> **Handler** 创建时就会采用当前的 **Looper** 来构建内部的消息循环系统，如果当前没有 **Looper** ,那么就会报错。

**怎么解决上述问题？两个方法：**

① 为当前线程创建 Looper 即可。
② 在 Looper 的线程中创建 Handler 也可以。

#### **工作原理：**

（1）**Handler 创建过程：** **Handler** 被创建之后，内部的 **MessageQueue** 和 **Looper** 就与 **Handler** 一起工作协同工作了， 然后通过 **Handler**  的 **post** 方法将一个 **Runnable** 投递给 **Handler** 内部的 **Looper** 去处理；也可以通过 **Handler** 的 **send** 方法发送一个消息，也是通过 **Looper** 去处理的，其实 **Post** 方法最终也是通过 **send** 方法来完成的..

（2）**send 方法的工作过程：**当 **Handler** 的 **send** 方法被调用时，它会调用 **MessageQueue** 的 **enqueueMessage** 方法将这个消息放到消息队列中，然后 **Looper** 发现新消息，就会处理这个消息，最终消息的 **Runnable** 或者 **Handler** 的 **handlerMessage** 方法就会被调用。

![handler](/images/handler.png)

## 二、 Android 的消息机制分析

### 1、消息队列的工作原理

> 消息队列在 **Android** 主要是指 **MessageQueue** ，**MessageQueue** 主要包括两个操作：插入和删除。消息队列的内部实现并不是队列，实际上通过一条单链表的数据结构来维护消息队列，单链表在插入删除上很有优势。

**① 插入（enqueueMessage）：**往消息队列中插入一条消息。（源码实现就是单链表的插入）

**② 删除（next）：**从消息队列中取出一条消息并将其从消息队列中移除。（next 是一个无限循环的方法，消息队列没有信息就处于阻塞状态，有新消息到来就执行单链表的删除）

### 2、Lopper 的工作原理


> **Looper** 在 **Android** 消息机制中扮演着消息循环的角色，作用：不停地从 **MessageQueue** 中查看是否有新的消息，如果有消息就会立刻处理，如果没有消息就会处于阻塞状态。

#### （1) Looper 的构造方法

① 创建一个 **MessageQueue** 消息队列。

② 将当前线程的对象保存起来。

#### （2）如何为一个线程创建 Looper

（Handle 的工作需要 Looper，没有 Looper 就会报错）

① 通过 **Looper.prepare()** 方法为线程创建一个 **Looper** 。

② 通过 **Looper.loop()** 方法来开启消息循环。

#### （3）创建线程的另一种方法

**① 主线程 Looper 的获取。** **Looper** 这个方法主要给线程也就是 **ActivityThread** 创建 **Looper** 使用的，本质也是通过 **prepare** 来实现的，由于主线程的 **Looper** 比较特殊，所以 **Looper** 提供了一个 **getMainLopper** 的方法获取主线程的 **Looper**。

**② Looper 的退出。**** Looper ** 提供了两个方法：**quit** 方法和 **quitSafely** 方法。

**两者区别：**

 -  **quite** 直接退出 **Looper**。
 -  而 **quitSafely** 只是设定一个退出标记，先把消息队列中的消息处理完之后再退出
    #### （4）Looper.loop() 方法实现原理
    **loop** 是一个死循环，唯一能跳出循环的方法就是 **MessageQueue** 的 **next** 方法返回了 **null**。当 **Looper** 的 **quit** 方法被调用时，**MessageQueue** 的 **quit** 方法或者 **quitSafely** 方法就会通知消息队列退出，当消息队列被标记为退出状态时，**next** 就会返回一个 **null**。**Looper** 是必须退出的，否则 **loop** 会永远循环下去。
    **loop** 方法会调用 **MessageQueue** 的 **next** 方法获取消息，如果 **MessageQueue** 没有消息，**next** 就会处于阻塞状态，**loop** 方法也会处于阻塞状态。

### 3、详解 Handler 的工作原理

> **Handler** 的主要工作就是发送和接收消息。消息的发送可以通过 **post** 一系列方法以及 **send** 的一系列方法来实现，**post** 的一系列方法最终都是通过 **send** 一系列方法来实现的。

**代码实现：**

```
public class HandlerActivity extends Activity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //开启线程
        handler();
    }
    //主线程
    Handler handler = new Handler(){

        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 1:
                    // 获取Message里面的复杂数据
                    Bundle date = new Bundle();
                    date = msg.getData();
                    String name = date.getString("name");
                    int age = date.getInt("age");
                    String sex = date.getString("sex");
                    //这里是主线程，可进行对UI的更新
                    textView.setText(name)
            }
        }
    };

    //子线程
    public void handler(){
        new Thread(new Runnable() {
            @Override
            public void run() {
                Message message = new Message();
                message.what = 1;

                // Message对象保存的数据是Bundle类型的
                Bundle data = new Bundle();
                data.putString("name", "李文志");
                data.putInt("age", 18);
                data.putString("sex", "男");
                // 把数据保存到Message对象中
                message.setData(data);
                // 使用Handler对象发送消息
                handler.sendMessage(message);
            }
        }).start();
    }
}
```

#### （1）发送消息

通过对源码分析，**Handler** 发送消息的过程仅仅是向消息队列中插入一条信息，**MessageQueue** 的 **next** 方法就会返回这条信息给 **Looper** ,**Looper** 接收到消息之后就立即处理，由 **Looper** 交给 **Handler** 去处理消息，**Handler** 的 **dispatchMessage** 方法就会被调用，这时候 **Handler** 就进入了消息处理阶段。

#### （2）消息处理

![这里写图片描述](https://img-blog.csdn.net/20180805112505780?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2OTAzMDQy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
**深入 dispatchMessage 的源代码进行分析，Handler 处理消息如下：**

① 首先检查 Message 的 callback 是否为 null, 不为 null 就通过 handlerCallback 来处理消息。（Message的 callback 是一个 Runnable d 对象，实际上就是 post 方法所递的 Runnable 参数）

②  其次检查 **mCallback** 是否为 **null**，不为 **null** 就调用 **mCallback** 的 **handlerMessage** 方法来处理消息。**Callback** 是个接口。

③ 我们通过 Callback 可以采用如下的方式来创建 Handle 对象。

```
Handler handler = new Handler(callback);
```
这样创建的意义就是创建一个实例但是并不需要派生 Handler 的子类。

③ 但是，在我们的日常开发中，经常派生一个 Handler 的子类并重写其 handleMessage 方法来处理具体的消息，如果不想创建派生子类，就可以通过 Callback 来实现。



### 4、主线程的消息循环

> **Android** 的主线程就是 **ActivityThread**，主线程的入口方法为 **main** ，在 **main** 方法中系统会通过 **Looper.prepareMainLooper()** ；来创建主线程的 **Looper** 以及 **MessageQueue** ，并通过 **Looper.loop()** 来开启主线程的消息循环。

主线程的消息循环开始了以后，**ActivityThread** 还需要一个 **Handler** 来和消息队列进行交互，这个 **Handler** 就是 **ActivityThread.H** 。**ActivityThread** 通过 **ApplicationThread** 和 **AMS** 进行进程间通信，**AMS** 以进程间通信的方式完成 **ActvityThread** 的请求后会回调 **ApplicationThread** 中的 **Binder** 方法，然后 **ApplicationThread** 向 **H** 发送消息，H 收到消息会将 **ApplicationThread** 中的逻辑切换到 **ActivityThread** 中去执行，即切换到主线程中去执行，这个过程就是主线程的消息循环模型。  

