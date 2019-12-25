---
title: Android 进阶之 Spinner 下拉框
categories: 
- Android 初级进阶
tags: 

- Android
---

![](\images\下拉框.jpg)

**Android** 中下拉框的使用是用 **Spinner** 来实现的，**Sprinner** 下拉框中的数据有两种添加方法。一种方法是使用 **spinner.xml** 布局文件的方式添加，另外一方式是用 **Adapter** 适配器的方法来实现的。之所以整理今天整理一下 **Spinner** 的两种方式的使用因为在实际的项目中都用到过，所以做了一个总结。 

<!--more-->



# Android 进阶之 Spinner 下拉框

## **Sprinner 的实现方式一：使用布局文件来传入数据。** 

### 步骤：

 **第一步：新建 activity_sprinner.xml 布局文件** 

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="xml布局引用方式:"
        android:textSize="20dp"/>
    <Spinner
        android:id="@+id/spinner_xml"
        android:layout_width="120dp"
        android:layout_height="wrap_content">
    </Spinner>
</LinearLayout>
```

**第二步：在 values 文件夹下新建 spinner.xml 文件。** 

```java
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="spingArr">
        <item>中国</item>
        <item>美国</item>
        <item>俄国</item>
    </string-array>
</resources>
```

**第三步：新建 SpinnerActivity.java 类。** 

```java
public class SpinnerActivity extends AppCompatActivity {

    //声明控件
    private Spinner spinner;

    private TextView textView;

    private ArrayAdapter arrayAdapter;

    private String spinnerStr;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.actvity_spinner);

        //关联控件
        spinner = (Spinner) findViewById(R.id.spinner_xml);
        textView = (TextView) findViewById(R.id.tv_text);

        //使数据布局文件和Adapter适配器关联
        relevanceSpinner();

        //Spinner下拉框监听事件
        spinner.setOnItemSelectedListener(new Spinner.OnItemSelectedListener() {
            //选择下拉框数据监听
            public void onItemSelected(AdapterView<?> arg0, View arg1,
                                       int arg2, long arg3) {
                spinnerStr = arrayAdapter.getItem(arg2).toString();
                textView.setText(spinnerStr);
            }
            //未选择下拉框监听
            public void onNothingSelected(AdapterView<?> arg0) {
                spinnerStr = "";
            }
        });
    }

    //使数据布局文件和Adapter适配器关联
    public void relevanceSpinner(){

        //R.array.spingArr 为 values 文件夹下 spinner.xml 中 string-array 的 name 值
        arrayAdapter = ArrayAdapter.createFromResource(this, R.array.spingArr, android.R.layout.simple_spinner_item);

        //将 adapter2 添加到 spinner 中
        spinner.setAdapter(arrayAdapter);
    }
}
```

**最终运行图示：** 

![](\images\spring.png)



## **Sprinner的实现方式二：在Activity中动态传入数据显示。** 

### 步骤

**步骤一：新建 activity_main.xml布局文件。** 

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:android="http://schemas.android.com/apk/res/android" >
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Activity引用方式:"
        android:textSize="20dp"/>
    <Spinner
        android:id="@+id/spinner_equment_name"
        android:layout_width="120dp"
        android:layout_height="wrap_content">
    </Spinner>
    <TextView
        android:id="@+id/tvText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</LinearLayout>
```

**步骤二：新建 MainActivity.java 类** 

```java
public class MainActivity extends AppCompatActivity {

    //声明控件
    private Spinner spinner;

    private ArrayAdapter arr_adapter = null;

    private List<String> list = new ArrayList<String>();

    private TextView text;

    private String spinnerText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //关联控件
        spinner = (Spinner) findViewById(R.id.spinner_equment_name);
        text = (TextView)findViewById(R.id.tvText);

        //list填充数据（如果是服务器接收的数据可动态填充）
        list.add("中国");
        list.add("美国");
        list.add("俄国");

        //适配器
        arr_adapter = new ArrayAdapter(this, android.R.layout.simple_spinner_item, list);

        //设置样式
        arr_adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);

        //加载spinner适配器
        spinner.setAdapter(arr_adapter);

        //Spinner 选择数据监听事件
        spinner.setOnItemSelectedListener(new Spinner.OnItemSelectedListener() {

            public void onItemSelected(AdapterView<?> arg0, View arg1,
                                       int arg2, long arg3) {
                spinnerText = arr_adapter.getItem(arg2).toString();
                text.setText(spinnerText);
            }
            public void onNothingSelected(AdapterView<?> arg0) {
                spinnerText = "";
                text.setText(spinnerText);
            }
        });
    }
}
```

**运行显示图示：** 

![](\images\spring2.png)

**还有一些关于Spinner的属性等到用到时在补充，以上就是这些，谢谢！** 