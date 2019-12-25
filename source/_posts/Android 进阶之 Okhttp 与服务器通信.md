---
title: Android 进阶之 OKHttp 与服务器通信
categories: 
- Android 初级进阶
tags: 

- Android 
---



![img](\images\心心相印.jpg) 

人生中第一次写博客，也就是大学大二期间。我认为记录这些点点滴滴的经验和知识有两点必要。第一，是能够记录自己在学习IT之路的一些经验和知识的整理；第二，把自己所理解的知识点更能详细分享给喜欢编程的每一个人，希望读者看了这些能够有所帮助，虽然别人也写到同样的知识，但是我通过学习这个知识点再加上我的个人理解来记录下来，有些知识说的不到位请各位批评改正！ 

<!-- more -->

​      言归正传，下面来讲一下利用okhttp做服务器通信（socket通信），之前大多数开发者用的是流的通信方式，自从安卓提供okhttp框架之后，实现客户端与服务器的通信更方便快捷了，废话不多说，下面我进行一一讲解！TT

**搭建OkHTTP框架之前注意：**

**1、在 build 中加入okhttp，Gson 的架包，修改 build.gradle（app）中加上：** 

```java
compile 'com.squareup.okhttp:okhttp:2.4.0'
```

**2、在 AndroidManifest 中增加请求网络的权限（如不加，APP连不上网络）；** 

```java
<uses-permission android:name="android.permission.INTERNET" />
```



## 搭建步骤：

 **1、首先创建okhttp对象，设置为全局（以下操作都围绕该对象来做的）**

```java
OkHttpClient okHttpClient = new OkHttpClient();
```

**2、对该对象进行封装（里边携带往服务器发送的相关参数（如果不是自己服务器参考该服务器开发文档）和URL（服务器提供的接口API））**

```java
Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                MediaType JSON = MediaType.parse("application/json; charset=utf-8");

                List<Map<String, String>> list = new ArrayList<>();//将JSON数据以Map形式存储到list中去,以List数组形式存储着数据;
                Map<String, String> map = new HashMap<>();        //建立Map对象，向Map添加数据;
                map.put("StartTime", "2018-01-11 09:14");
                map.put("EndTime", "");
                map.put("Interval", "5");
                map.put("Start", "0");
                map.put("Limit", "1");
                map.put("Order", "1");
                list.add(map);
                JSONObject jsonObject = new JSONObject(map);        //创建JSONObject对象;

                String string = jsonObject.toString();              //将jsonObject对象转换成字符串;
                RequestBody body = RequestBody.create(JSON, string);//将JSON数据打包成Body通过post上传;
                Request request = new Request.Builder()
                        .url(Get_All_url)                           //该服务器的api（URL）
                        .addHeader("userkey", "55656535336494b84c749b31453ea55")
                        .post(body)
                        .build();
                executeRequest(request);
            }
        });
        t1.start();
```

做服务器连接是一个耗时的工作要在线程里完成，这里我将发往服务器的相关参数封装到 **Map** 里边然后将 **Map** 放到 **List** 中将 **List** 打包成 **JSONobject** 对象，然后将对象转化成字符串才能封装成传输数据的 **body** 才能上传服务器，**body** “相当于人的身体”，将其封装到 **request** 对象中，如果相关服务器有相关的 **key** 标识的话，可以通过 **addHeader** 键值对方式存到 **request** 头部，这样一个完整的数据包打包成功了，可以通 **executeRequest(request)**发送到服务器！ 

> **补充：在以上封装数据的时候可能与别的博主写的不同，但是用这个List/map的格式封装感觉更易懂一些，下面就详细说一下。** 

**我们通常看到别人的 okhttp 上传都是用到的 FormEncodingBuilder 可以简单理解为表单的形式，代码如下：** 

```java
FormEncodingBuilder builder = new FormEncodingBuilder ();
builder.add("username","admin");
builder.ass("password","123");
Request request = new Request.Builder()
                        .url(https://mp.csdn.net/)                 //该服务器的api（URL）
                        .addHeader("userkey", "2312333321323")     //头部标识（不需要可去掉）
                        .post(builder.builde())
                        .build();
                executeRequest(request);
```

**我们都知道网络通信的格式大多数 Json 数据，我们要利用 map 和 List 的结合打包成 Json 数据，下面举个例子,比如我们要打包成这种形式的 Json 数据：**

![img](\images\json.png) 

```java
                Map<String, String> map1 = new HashMap<>();//生成一个Map类型对象map1                
                Map<String, String> map2 = new HashMap<>();//生成一个Map类型对象map2
                map1.add("Name","T1");
                map1.add("Value","1");
                map2.add("Name","01H1");
                map2.add("Value","96.2")；
```

在 **Json** 数据中一个{}里的内容就称为一个对象，**{"Name":"T1","Value","1"} **称为一个对象，我们将这个对象数据以 **Map** 键值对类型存到 **map1** 中，同理将第二个 **Json** 对象存放到 **map2** 中去，**map1、map2**就可以封装成**{"Name":"T1","Value","1"}，{"Name":"01H1","Value","96.2"} **的形式，我们可以清楚的看到两个{}对象最外层还包含着一个 **[ ]** 的括号，我们就可以将 **[ ]** 用 **List** 来表示，那 **List** 存放的数据类型是什么呢？不明思义，当然是{}两个 j**son** 对象咯，**{ }** 两个对象用 **map** 封装的，所以 **List** 中存放着 **Map** 类型的数据，我们可以这样声明并添加：

```java
  List<Map<String, String>> list = new ArrayList<>();
  //我们将封装好的map1和map2添加到list中去
  list.add(map1);
  list.add(map2);
  //我们将list装换成JsonArray数组形式
  JsonArray jsonarray = new JSONArray(list);
  //我们再把jsonarray形式转化成字符串就可以上传了
  String str = jsonarray.toString();
  RequestBody body = RequestBody.create(JSON, str);
```

经过以上的层层封装，我们终于把数据封装成想要的样子，简单总结一下， **{ }** 的数据用 **Map** 键值对进行封装，而 **[] Json** 数组形式用 **List** 封装，最后那 List 转化成 JsonArray 数组形式就可以得到想要的数据了！ 注意：如果**Json** 数据只由 **{}**  对象形式封装成的，我们可以将 **map** 转化成 **JsonObject** 对象,然后将 **JsonObject** 妆化成字符串上传服务器！ 

```java
JsonObject jsonobject = new JSONObject(map1);
String str = new jsonobjct.toString();
```

**经过上边的讲解你是否可以封装任意 Json 数据了呢？** 



**3、第三步是来对服务器返回的数据做处理**

该 OKHTTP 框架也提供了简单的服务器回调方法，下边我们来看一下

```java
//服务器返回调用函数
private void executeRequest(Request request) {

    //3.将Request封装为Call
    Call call = new OkHttpClient().newCall(request);
    //4.执行call
    call.enqueue(new Callback() {

        @Override//回调错误时
        public void onFailure(Request request, IOException e) {
            
        }

        @Override//回调成功时
        public void onResponse(Response response) throws IOException {
            final String relsult = response.body().string();//接收服务器返回来的信息
             try {
               JSONArray jsonarray = new JSONArray(relsult);//将返回的信息转换成JSON形式
                       
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
    });
}
```

当服务器进行响应时，会自动的调用 **executeRequest()** 函数，回调函数中自带的两种相应方法分别为服务器响应失败和服务器响应成功，开发人员可以在其中做相应的解析和回调！ 