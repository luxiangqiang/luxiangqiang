---
title: Android 进阶之百度推送
categories: 
- Android 初级进阶
tags: 

- Android
---

![](\images\百度推送.jpg)

小鹿带你学会百度推送！

<!--more-->

# Android 进阶之百度推送

### 步骤

**第一步：首先在百度云注册账号** （[](http://push.baidu.com/)）

 **第二步：注册好账号之后，创建应用。** 

![](\images\B1.png)

**第三步：点击创建新应用**。 

![](\images\B2.png)

**第四步：填写应用名称** 

![](\images\B3.png)

**第五步：点击创建好应用之后进行应用配置。** 

![](\images\B4.png)

**第六步：选择终端（这里我们选择 Android），将项目包名填写进去。** 

![](\images\B5.png)

**第七步：要记住 API KEY 项目中要用到。** 

![](\images\B6.png)

**第八步：下载百度推送 SDK（官网），添加到项目中。** 

![](\images\B7.png)

**第九步：开始新建 Android 项目为 BaiDu_Push_Demo。**

**第十步：在 build.gradle 中添加依赖。**

```java
//加载jar包
compile files('src/main/JniLibs/pushservice-6.1.1.21.jar')
```

**第十一步：新建 PushServiceReceiver.java 类（类中都是关于百度推送的回调）。** 

```java
package com.example.boybaby.baidu_pust_demo;

/**
 * Created by apple on 2018/4/25.
 */

import android.content.Context;
import android.text.TextUtils;
import android.util.Log;
import android.widget.Toast;

import org.json.JSONException;
import org.json.JSONObject;

import java.util.List;

/*
 * Push消息处理receiver。请编写您需要的回调函数， 一般来说： onBind是必须的，用来处理startWork返回值；
 *onMessage用来接收透传消息； onSetTags、onDelTags、onListTags是tag相关操作的回调；
 *onNotificationClicked在通知被点击时回调； onUnbind是stopWork接口的返回值回调
 * 返回值中的errorCode，解释如下：
 *0 - Success
 *10001 - Network Problem
 *10101  Integrate Check Error
 *30600 - Internal Server Error
 *30601 - Method Not Allowed
 *30602 - Request Params Not Valid
 *30603 - Authentication Failed
 *30604 - Quota Use Up Payment Required
 *30605 -Data Required Not Found
 *30606 - Request Time Expires Timeout
 *30607 - Channel Token Timeout
 *30608 - Bind Relation Not Found
 *30609 - Bind Number Too Many
 * 当您遇到以上返回错误时，如果解释不了您的问题，请用同一请求的返回值requestId和errorCode联系我们追查问题。
 *
 */

public class PushMessageReceiver extends com.baidu.android.pushservice.PushMessageReceiver {
    /**
     * TAG to Log
     */
    public static final String TAG = PushMessageReceiver.class
            .getSimpleName();

    /**
     * 调用PushManager.startWork后，sdk将对push
     * server发起绑定请求，这个过程是异步的。绑定请求的结果通过onBind返回。 如果您需要用单播推送，需要把这里获取的channel
     * id和user id上传到应用server中，再调用server接口用channel id和user id给单个手机或者用户推送。
     *
     * @param context   BroadcastReceiver的执行Context
     * @param errorCode 绑定接口返回值，0 - 成功
     * @param appid     应用id。errorCode非0时为null
     * @param userId    应用user id。errorCode非0时为null
     * @param channelId 应用channel id。errorCode非0时为null
     * @param requestId 向服务端发起的请求id。在追查问题时有用；
     * @return none
     */
    @Override
    public void onBind(Context context, int errorCode, String appid,
                       String userId, String channelId, String requestId) {
        String responseString = "onBind errorCode=" + errorCode + " appid="
                + appid + " userId=" + userId + " channelId=" + channelId
                + " requestId=" + requestId;
        Log.d(TAG, responseString);

        if (errorCode == 0) {
            // 绑定成功
            Log.d(TAG, "绑定成功");
        }
    }

    /**
     * 接收透传消息的函数。
     *
     * @param context             上下文
     * @param message             推送的消息
     * @param customContentString 自定义内容,为空或者json字符串
     */
    @Override
    public void onMessage(Context context, String message,
                          String customContentString) {
        String messageString = "透传消息 onMessage=\"" + message
                + "\" customContentString=" + customContentString;
        Log.d(TAG, messageString);

        // 自定义内容获取方式，mykey和myvalue对应透传消息推送时自定义内容中设置的键和值
        if (!TextUtils.isEmpty(customContentString)) {
            JSONObject customJson = null;
            try {
                customJson = new JSONObject(customContentString);
                String myvalue = null;
                if (!customJson.isNull("mykey")) {
                    myvalue = customJson.getString("mykey");
                }
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }

    }

    /**
     * 接收通知到达的函数。
     *
     * @param context             上下文
     * @param title               推送的通知的标题
     * @param description         推送的通知的描述
     * @param customContentString 自定义内容，为空或者json字符串
     */

    @Override
    public void onNotificationArrived(Context context, String title,
                                      String description, String customContentString) {

        String notifyString = "通知到达 onNotificationArrived  title=\"" + title
                + "\" description=\"" + description + "\" customContent="
                + customContentString;
        Log.d(TAG, notifyString);
        //Toast.makeText(context,description,Toast.LENGTH_LONG).show();
        //Intent intent=new Intent(context,Main2Activity.class);
        //context.startActivity(intent);
        // 自定义内容获取方式，mykey和myvalue对应通知推送时自定义内容中设置的键和值
        if (!TextUtils.isEmpty(customContentString)) {
            JSONObject customJson = null;
            try {
                customJson = new JSONObject(customContentString);
                String myvalue = null;
                if (!customJson.isNull("mykey")) {
                    myvalue = customJson.getString("mykey");
                }
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        // Demo更新界面展示代码，应用请在这里加入自己的处理逻辑
        // 你可以參考 onNotificationClicked中的提示从自定义内容获取具体值
    }

    /**
     * 接收通知点击的函数。
     *
     * @param context             上下文
     * @param title               推送的通知的标题
     * @param description         推送的通知的描述
     * @param customContentString 自定义内容，为空或者json字符串
     */
    @Override
    public void onNotificationClicked(Context context, String title,
                                      String description, String customContentString) {
        String notifyString = "通知点击 onNotificationClicked title=\"" + title + "\" description=\""
                + description + "\" customContent=" + customContentString;
        Log.d(TAG, notifyString);
        Toast.makeText(context,description,Toast.LENGTH_LONG).show();
        // 自定义内容获取方式，mykey和myvalue对应通知推送时自定义内容中设置的键和值
        if (!TextUtils.isEmpty(customContentString)) {
            JSONObject customJson = null;
            try {
                customJson = new JSONObject(customContentString);
                String myvalue = null;
                if (!customJson.isNull("mykey")) {
                    myvalue = customJson.getString("mykey");
                }
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }

    }

    /**
     * setTags() 的回调函数。
     *
     * @param context   上下文
     * @param errorCode 错误码。0表示某些tag已经设置成功；非0表示所有tag的设置均失败。
     * @param failTags  设置失败的tag
     * @param requestId 分配给对云推送的请求的id
     */
    @Override
    public void onSetTags(Context context, int errorCode,
                          List<String> sucessTags, List<String> failTags, String requestId) {
        String responseString = "onSetTags errorCode=" + errorCode
                + " sucessTags=" + sucessTags + " failTags=" + failTags
                + " requestId=" + requestId;
        Log.d(TAG, responseString);

    }

    /**
     * delTags() 的回调函数。
     *
     * @param context   上下文
     * @param errorCode 错误码。0表示某些tag已经删除成功；非0表示所有tag均删除失败。
     * @param failTags  删除失败的tag
     * @param requestId 分配给对云推送的请求的id
     */
    @Override
    public void onDelTags(Context context, int errorCode,
                          List<String> sucessTags, List<String> failTags, String requestId) {
        String responseString = "onDelTags errorCode=" + errorCode
                + " sucessTags=" + sucessTags + " failTags=" + failTags
                + " requestId=" + requestId;
        Log.d(TAG, responseString);

    }

    /**
     * listTags() 的回调函数。
     *
     * @param context   上下文
     * @param errorCode 错误码。0表示列举tag成功；非0表示失败。
     * @param tags      当前应用设置的所有tag。
     * @param requestId 分配给对云推送的请求的id
     */
    @Override
    public void onListTags(Context context, int errorCode, List<String> tags,
                           String requestId) {
        String responseString = "onListTags errorCode=" + errorCode + " tags="
                + tags;
        Log.d(TAG, responseString);

    }

    /**
     * PushManager.stopWork() 的回调函数。
     *
     * @param context   上下文
     * @param errorCode 错误码。0表示从云推送解绑定成功；非0表示失败。
     * @param requestId 分配给对云推送的请求的id
     */
    @Override
    public void onUnbind(Context context, int errorCode, String requestId) {
        String responseString = "onUnbind errorCode=" + errorCode
                + " requestId = " + requestId;
        Log.d(TAG, responseString);

        if (errorCode == 0) {
            // 解绑定成功
            Log.d(TAG, "解绑成功");
        }
    }


}
```

**第十二步：在 AndroidManifest.xml 中配置权限和相关服务。** 

```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.boybaby.baidu_pust_demo">

    <!-- Push service 运行需要的权限 -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <!-- 富媒体需要声明的权限 -->
    <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
    <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
    <uses-permission android:name="android.permission.EXPAND_STATUS_BAR" />

    <!-- 适配Android N系统必需的ContentProvider写权限声明，写权限包含应用包名 -->
    <uses-permission android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.com.example.boybaby.com.example.boybaby.baidu_pust_demo" />

    <permission
        android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.com.example.boybaby.com.example.boybaby.baidu_pust_demo"
        android:protectionLevel="signature">
    </permission>

    <!-- 权限结束 -->
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!-- push service start -->
        <!-- 用于接收系统消息以保证PushService正常运行 -->
        <receiver
            android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
                <action android:name="com.baidu.android.pushservice.action.media.CLICK" />
                <!-- 以下四项为可选的action声明，可大大提高service存活率和消息到达速度 -->
                <action android:name="android.intent.action.MEDIA_MOUNTED" />
                <action android:name="android.intent.action.USER_PRESENT" />
                <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
                <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
            </intent-filter>
        </receiver>
        <!-- Push服务接收客户端发送的各种请求 -->
        <receiver
            android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED" />

                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

        <!-- 4.4版本新增的CommandService声明，提升小米和魅族手机上的实际推送到达率 -->
        <service
            android:name="com.baidu.android.pushservice.CommandService"
            android:exported="true" />

        <!-- 适配Android N系统必需的ContentProvider声明，写权限包含应用包名 -->
        <provider
            android:name="com.baidu.android.pushservice.PushInfoProvider"
            android:authorities="com.example.boybaby.baidu_pust_demo.bdpush"
            android:exported="true"
            android:protectionLevel="signature"
            android:writePermission="baidu.push.permission.WRITE_PUSHINFOPROVIDER.com.example.boybaby.baidu_pust_demo.PushMessageReceiver" />

        <!-- push应用定义消息receiver声明 -->
        <receiver android:name="com.example.boybaby.baidu_pust_demo.PushMessageReceiver">
            <intent-filter>

                <!-- 接收push消息 -->
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <!-- 接收bind、setTags等method的返回结果 -->
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <!-- 接收通知点击事件，和通知自定义内容 -->
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>
        <!-- push结束 -->

    </application>

</manifest>
```

**注意：在配置文件中需要将包名换成自己项目的包名。** 

**①** 

![](\images\B8.png)

**②** 

![](\images\B9.png)

③

![](\images\B10.png)

**以上三个地方的包名需要改成自己的。**

**第十三步：在 MainAcitivity 中注册百度推送 API KEY。**

```java
package com.example.boybaby.baidu_pust_demo;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;

import com.baidu.android.pushservice.PushConstants;
import com.baidu.android.pushservice.PushManager;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //注册,第三个参数是要修改的API KEY的字符串
        PushManager.startWork(getApplicationContext(), PushConstants.LOGIN_TYPE_API_KEY,"Cy1dU6vskzHqPmEmNu2aCZsR");
    }
}
```

**第十四步：创建通知并发送。** 

![](\images\B11.png)

**第十五步：发送成功。** 

![](\images\B12.png)

**手机端收到通知：** 

![](\images\B13.png)