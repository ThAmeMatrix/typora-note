# [实例] Android-MQTT 应用

[TOC]

## 实验目的:

MQTT 作为物联网传输协议,具有低带宽、低耗能、低成本、可靠传输的特点,已被广泛应用于移动即时消息通信,物联网,智能硬件领域。对于 Android等终端设备, 其在计算资源以及能耗方面受到一定限制, mqtt 在移动终端中的应用具有明显的优势。
本次实验将使用 MQTT 开发 Android 程序, 实现移动终端消息的订阅与发送。

注: ~~这份MQTT的demo源码来自实验室的代代相传, 使用非常精简的语句实现了 MQTT 通讯的基本功能(佩服), 可在这份代码的基础上添加数据可视化, 数据存储等功能.~~ 



## 实验软件:

​    **Android Studio 2.3+**

​    **hiveMQ** (搭建在云服务器上)

​    **Chrome 插件 MQTTLens**



## 实验步骤:

### 1. 创建 Android 工程

​	在 Android Studio 中创建工程“MQTTExample”, 选择目标设备为“Phone and Tablet”。



### 2. 添加MQTT依赖

本实验使用 Eclipse paho MQTT Client , 在新建 module(app) 的 build.gradle 文件中添加 mqtt jar 依赖:

```json
dependencies {
	...
	compile 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.1.1'
}

```



### 3. 添加网络访问权限

在使用 mqtt 时需要使用网络连接, 因此需要给 android 应用添加网络访问权限. 在 AndroidManifest.xml 中添加d代码:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

添加后的 AndroidManifest.xml 文件如下:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.zucc.g3.hzy.myapplication">
    <uses-permission android:name="android.permission.INTERNET" />
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
    </application>

</manifest>
```



### 4. 编写 Activity 界面布局

在步骤 1 中,通过 Android Studio 向导创建主 Activity,对应 Layout 名称为 activity_main。在 layout 目录下打开编辑 activity_main.xml 文件,创建用于发送、接收 MQTT 消息的界面。

具体实现代码:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout  xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="PubTopic"/>
        <EditText
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/pubTopic"
            android:lines="1"
            android:layout_weight="1"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="PubMsg"/>
        <EditText
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/pubMessage"
            android:lines="1"
            android:layout_weight="1"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/pubButton"
            android:text="pub"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:orientation="horizontal">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="SubTopic"/>
        <EditText
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/subTopic"
            android:lines="1"
            android:layout_weight="1"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/subButton"
            android:text="sub"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:orientation="horizontal">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="SubMsg"/>
        <ScrollView
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_marginLeft="5dp"
            android:layout_weight="1">
            <TextView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:id="@+id/submessage"/>
        </ScrollView>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:gravity="right"
        android:layout_height="wrap_content">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="clear"
            android:id="@+id/clearButton"/>
    </LinearLayout>

</LinearLayout>
```

实现的结果如下:

![选区_195](/home/nicholascheng/Pictures/ScreenShots/选区_195.png)



### 5. 编辑 MainActivity

修改 MainActivity 文件, 实现界面交互以及 MQTT 消息的转发

1. 定义常量

在 onCreate 函数上方, 添加如下常量:

```java
private MqttAsyncClient mqttClient;

private final static String host = "123.206.127.199:1883";
private final static String username = "";
private final static String password = "";

private final static int CONNECTED = 1;
private final static int LOST = 2;
private final static int FAIL = 3;
private final static int RECEIVE = 4;
private EditText pubTopic, pubMsg, subTopic;
private TextView subMsg;
private Button pubButton, subButton, clearButton;
```

2. 获取界面组件

在 onCreate 函数中使用 findViewById() 方法获取所有使用的组件:

```java
pubTopic = findViewById(R.id.pubTopic);
pubMsg = findViewById(R.id.pubMessage);
subTopic = findViewById(R.id.subTopic);
subMsg = findViewById(R.id.submessage);
pubButton = findViewById(R.id.pubButton);
subButton = findViewById(R.id.subButton);
clearButton = findViewById(R.id.clearButton);
```

3. 添加按键事件

MainActivity 实现 Button.OnClickListener 接口并重写 onClick 方法:

```java
public void onClick(View v) {
    if(v==pubButton){
        try{
            mqttClient.publish(pubTopic.getText().toString(),
                    pubMsg.getText().toString().getBytes(),1,false);
        } catch (MqttException e) {
            e.printStackTrace();
        }
    }else if(v==subButton){
        try {
            mqttClient.subscribe(subTopic.getText().toString(),1);
        } catch (MqttException e) {
            e.printStackTrace();
        }
    }else if(v==clearButton){
        subMsg.setText("");
    }
}
```

同时, 在 onCreate 函数中设置按钮监听:

```java
pubButton.setOnClickListener(this);
subButton.setOnClickListener(this);
clearButton.setOnClickListener(this);
```



### 6. MQTT 连接

1. 创建 MQTT 客户端并连接服务器

```java
    private void connectBroker() {
        try {
            mqttClient = new MqttAsyncClient("tcp://" + host, 
                    "ClientID" + Math.random(), new MemoryPersistence());
        } catch (MqttException e) {
            e.printStackTrace();
        }
    }
```

2. 连接服务器并监听连接事件

在连接服务器前需要对客户端做连接配置,例如连接服务器账号密码、连接超时、心跳间隔等, 客户端设置使用单独的类 MqttConnectOptions。

```java
private MqttConnectOptions getOptions() {

    MqttConnectOptions options = new MqttConnectOptions();
    options.setCleanSession(true);//重连不保持状态
    if (username != null && username.length() > 0 && password != null && password.length() > 0) {
        options.setUserName(username);//设置服务器账号密码
        options.setPassword(password.toCharArray());
    }
    options.setConnectionTimeout(10);//设置连接超时时间
    options.setKeepAliveInterval(30);//设置保持活动时间，超过时间没有消息收发将会触发ping消息确认
    return options;
}
```

代码中使用了 Mqtt 异步客户端,需要使用 IMqttActionListener 实现类监听连接状态。

```java
private IMqttActionListener mqttActionListener = new IMqttActionListener() {
    @Override
    public void onSuccess(IMqttToken asyncActionToken) {
        //连接成功处理
        Message msg = new Message();
        msg.what = CONNECTED;
        handler.sendMessage(msg);
    }

    @Override
    public void onFailure(IMqttToken asyncActionToken, Throwable exception) {
        exception.printStackTrace();
        //连接失败处理
        Message msg = new Message();
        msg.what = FAIL;
        handler.sendMessage(msg);
    }
};
```

客户端使用 options 连接服务器并添加监听:

```java
mqttClient.connect(getOptions(), null, mqttActionListener);
```
3. 消息回调

客户端连接后,可以通过添加 callback 对象监听消息收发,连接断开事件。

定义 MqttCallback 对象并设置客户端回调:

```java
    private MqttCallback callback=new MqttCallback() {
        @Override
        public void connectionLost(Throwable cause) {
            //连接断开
            Message msg=new Message();
            msg.what=LOST;
            handler.sendMessage(msg);
        }

        @Override
        public void messageArrived(String topic, MqttMessage message) throws Exception {
            //消息到达
//            subMsg.append(new String(message.getPayload())+"\n"); //不能直接修改,需要在UI线程中操作
            Message msg=new Message();
            msg.what=RECEIVE;
            msg.obj=new String(message.getPayload())+"\n";
            handler.sendMessage(msg);
        }

        @Override
        public void deliveryComplete(IMqttDeliveryToken token) {
            //消息发送完成
        }
    };
```
客户端设置回调方法:

```java
mqttClient.setCallback(callback);
```

添加连接options和回调方法后, 最终的客户端代码:

```java
    private void connectBroker() {
        try {
            mqttClient = new MqttAsyncClient("tcp://" + host,
                    "ClientID" + Math.random(), new MemoryPersistence());
//            mqttClient.connect(getOptions());
            mqttClient.connect(getOptions(), null, mqttActionListener);
            mqttClient.setCallback(callback);
        } catch (MqttException e) {
            e.printStackTrace();
        }
    }
```

至此,Android MQTT 框架已经完成,后续可添加界面按键事件处理。



### 8. 消息订阅与接收

定义 Handler,处理接收到消息:

```java
private Handler handler=new Handler(){
    @Override
    public void handleMessage(Message msg){
        if(msg.what==CONNECTED){
            Toast.makeText(MainActivity.this,"连接成功",Toast.LENGTH_SHORT).show();
        }else if(msg.what==LOST){
            Toast.makeText(MainActivity.this,"连接丢失，进行重连",Toast.LENGTH_SHORT).show();
        }else if(msg.what==FAIL){
            Toast.makeText(MainActivity.this,"连接失败",Toast.LENGTH_SHORT).show();
        }else if(msg.what==RECEIVE){
            subMsg.append((String)msg.obj);
        }
        super.handleMessage(msg);
    }
};
```

最后, 在 onCreate() 函数中添加连接MQTT服务器并进行消息处理的语句:

```java
connectBroker();
```



完整的 MainActivity 代码:

```java
//package com.example.nicholascheng.simplemqtt;		你自己的包名

import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import org.eclipse.paho.client.mqttv3.IMqttActionListener;
import org.eclipse.paho.client.mqttv3.IMqttDeliveryToken;
import org.eclipse.paho.client.mqttv3.IMqttToken;
import org.eclipse.paho.client.mqttv3.MqttAsyncClient;
import org.eclipse.paho.client.mqttv3.MqttCallback;
import org.eclipse.paho.client.mqttv3.MqttConnectOptions;
import org.eclipse.paho.client.mqttv3.MqttException;
import org.eclipse.paho.client.mqttv3.MqttMessage;
import org.eclipse.paho.client.mqttv3.persist.MemoryPersistence;

public class MainActivity extends AppCompatActivity implements View.OnClickListener{

    private MqttAsyncClient mqttClient;

    private final static String host = "123.206.127.199:1883";
    private final static String username = "";
    private final static String password = "";
    private final static int CONNECTED = 1;
    private final static int LOST = 2;
    private final static int FAIL = 3;
    private final static int RECEIVE = 4;
    private EditText pubTopic, pubMsg, subTopic;
    private TextView subMsg;
    private Button pubButton, subButton, clearButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        pubTopic = findViewById(R.id.pubTopic);
        pubMsg = findViewById(R.id.pubMessage);
        subTopic = findViewById(R.id.subTopic);
        subMsg = findViewById(R.id.submessage);
        pubButton = findViewById(R.id.pubButton);
        subButton = findViewById(R.id.subButton);
        clearButton = findViewById(R.id.clearButton);
        pubButton.setOnClickListener(this);
        subButton.setOnClickListener(this);
        clearButton.setOnClickListener(this);
        connectBroker();
    }

    private void connectBroker() {
        try {
            mqttClient = new MqttAsyncClient("tcp://" + host,
                    "ClientID" + Math.random(), new MemoryPersistence());
            mqttClient.connect(getOptions(), null, mqttActionListener);
            mqttClient.setCallback(callback);
        } catch (MqttException e) {
            e.printStackTrace();
        }
    }

    private MqttConnectOptions getOptions() {

        MqttConnectOptions options = new MqttConnectOptions();
        options.setCleanSession(true);//重连不保持状态
        if (username != null && username.length() > 0 && password != null && password.length() > 0) {
            options.setUserName(username);//设置服务器账号密码
            options.setPassword(password.toCharArray());
        }
        options.setConnectionTimeout(10);//设置连接超时时间
        options.setKeepAliveInterval(30);//设置保持活动时间，超过时间没有消息收发将会触发ping消息确认
        return options;
    }

    private IMqttActionListener mqttActionListener = new IMqttActionListener() {
        @Override
        public void onSuccess(IMqttToken asyncActionToken) {
            //连接成功处理
            Message msg = new Message();
            msg.what = CONNECTED;
            handler.sendMessage(msg);
        }

        @Override
        public void onFailure(IMqttToken asyncActionToken, Throwable exception) {
            exception.printStackTrace();
            //连接失败处理
            Message msg = new Message();
            msg.what = FAIL;
            handler.sendMessage(msg);
        }
    };

    private MqttCallback callback=new MqttCallback() {
        @Override
        public void connectionLost(Throwable cause) {
            //连接断开
            Message msg=new Message();
            msg.what=LOST;
            handler.sendMessage(msg);
        }

        @Override
        public void messageArrived(String topic, MqttMessage message) throws Exception {
            //消息到达
//            subMsg.append(new String(message.getPayload())+"\n"); //不能直接修改,需要在UI线程中操作
            Message msg=new Message();
            msg.what=RECEIVE;
            msg.obj=new String(message.getPayload())+"\n";
            handler.sendMessage(msg);
        }

        @Override
        public void deliveryComplete(IMqttDeliveryToken token) {
            //消息发送完成
        }
    };

    private Handler handler=new Handler(){
        @Override
        public void handleMessage(Message msg){
            if(msg.what==CONNECTED){
                Toast.makeText(MainActivity.this,"连接成功",Toast.LENGTH_SHORT).show();
            }else if(msg.what==LOST){
                Toast.makeText(MainActivity.this,"连接丢失，进行重连",Toast.LENGTH_SHORT).show();
            }else if(msg.what==FAIL){
                Toast.makeText(MainActivity.this,"连接失败",Toast.LENGTH_SHORT).show();
            }else if(msg.what==RECEIVE){
                subMsg.append((String)msg.obj);
            }
            super.handleMessage(msg);
        }
    };

    public void onClick(View v) {
        if(v==pubButton){
            try{
                mqttClient.publish(pubTopic.getText().toString(),
                        pubMsg.getText().toString().getBytes(),1,false);
            } catch (MqttException e) {
                e.printStackTrace();
            }
        }else if(v==subButton){
            try {
                mqttClient.subscribe(subTopic.getText().toString(),1);
            } catch (MqttException e) {
                e.printStackTrace();
            }
        }else if(v==clearButton){
            subMsg.setText("");
        }
    }

}
```



## 实验结果测试:

使用 Chrome 插件 MQTTLens 连接 MQTT 服务器,在服务其中订阅某Topic。

![选区_196](/home/nicholascheng/Pictures/ScreenShots/选区_196.png)

将移动终端作为发送者, MQTTLens 作为接收者,  通过移动终端发送同一Topic 的内容到同一服务器,查看MQTTLens 是否能接收数据。
同理,可以使用 MQTTLens 作为发送者, 移动终端作为接收者, 查看移动终端是否能接收 MQTTLens 发送的数据。

当然, 如果你已经配置好了 Node-RED 的环境, 使用 Node-RED 的 mqtt 节点进行测试是更方便的选择:

![选区_204](/home/nicholascheng/Pictures/ScreenShots/选区_204.png)

![选区_205](/home/nicholascheng/Pictures/ScreenShots/选区_205.png)



## 实验拓展:

### 1. 如何处理同时接收到的多个数据?

若希望处理同时接收到的多个数据, 如(DHT11发送某一时刻的温度与湿度), 可以使用 `Gson` 包实现 JSON 字符串与 Java 对象之间的转换, 完成对多个数据的解析.

Gson 使用方法:

1. 添加依赖

```json
dependencies {
    compile 'com.google.code.gson:gson:2.8.5'
}
```

2. 定义 java 类


```java
public class User{
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

3. 使用 gson 解析 Json 返回 java 对象

若接受到的信息为: `{"name":"Jerry", "age":24}`


```java
User user = gson.fromJson(msg.obj.toString(), User.class);
System.out.println(user.getName() + "\n");
System.out.println(String.valueOf(user.getAge()) + "\n");
// Jerry
// 24
```



### 2. 如何实现数据可视化?

可使用 `MPAndroidChart`,  `WilliamChart`, `HelloChart` 等 Android 开源图表控件:

![选区_199](/home/nicholascheng/Pictures/ScreenShots/选区_199.png)

![HelloChart](/home/nicholascheng/Pictures/ScreenShots/选区_197.png)

在 Github 上可找到项目地址, 添加依赖后按照教程在布局中添加图表. 图表初始是静态的, 将接受到的数据作为图表值, 才可成为动态图表实现数据可视化.



### 3. 如何实现数据存储?

**SQLite** 是一个关系数据库管理系统，它包含在一个相对小的**C程序库**中. 与许多其它数据库管理系统不同，SQLite不是一个**客户端/服务器**结构的数据库引擎，而是被集成在用户程序中。

Android 系统自带了 SQLite 数据库, 这是一个小巧的嵌入式数据库, 使用简单, 开发方便, 多数 sql 语句和 oracle 一样. ~~然而大一并没有教过SQL语句, 所以做数据存储有点难度...~~

推荐教程:  https://developer.android.com/training/data-storage/sqlite#java

