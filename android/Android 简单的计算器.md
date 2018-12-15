# [实例] Android 简单的计算器

[TOC]

## 概述

本实例的代码：https://github.com/Ubilabs-NicholasCheng/Caculatar-test/tree/master

制作一个简单计算器是 Android 的经典实例，可以让开发者体会到 Android 前后端分离开发的特点与优势。内容主要包括 Android 的简（jian）单（lou）前端设计，前后端的组件如何连接，以及后端如何进行逻辑处理等。

下面，我们就开始吧！

## 新建项目

首先，在 Android Studio 中新建项目，项目名：Calculator-test，在会生成两个文件：activity_main.xml 与 MainActivity.java, 分别是前端的布局定义文件, 和后端的活动文件。

制作流程可分为三步：1.布置前端控件 -> 2. 完成后端逻辑 -> 3. 美化前端界面：

##布置控件

> **注：在 activity_main.xml 文件中**

需要的UI控件：

- EditText：允许用户在控件中输入和编辑内容，并在程序中对这些内容进行处理
- TextView：用于显示一段文本信息
- Button：用于和用户交互的一个重要控件，监听事件以触发特定的动作

界面布局的功能如下：

- EditText1：输入操作数1
- EditText2：输入操作数2
- TextView：显示结果
- ButtonAdd：对操作数1与操作数2进行加法运算
- ButtonSubtract：对操作数1与操作数2进行减法运算
- ButtonMultiply：对操作数1与操作数2进行乘法运算
- ButtonDivide：对操作数1与操作数2进行除法运算
- ButtonClear：清空EditText的内容，初始化TextView

Android的界面有两种编辑模式：Design与Text，即图形化编辑模式和代码编辑模式，可在编辑界面的左下角切换

一般推荐在Design中完成大致结构的布置，再在Text中微调控件的各种属性：

![选区_136](/home/nicholascheng/Pictures/ScreenShots/选区_136.png)

> activity_main.xml的完整代码：https://github.com/Ubilabs-NicholasCheng/SimpleCalculator/blob/6868921846cbd192a8430dc52ae036859dd12aff/app/src/main/res/layout/activity_main.xml



##编写后台逻辑 

> **注：在 MainActivity.java 中**

####新建全局变量：

```java
EditText num1, num2;
TextView result;
Button add, subtract, multiply, divide, clear;
```

####连接控件：

```java
num1 = findViewById(R.id.num1);
num2 = findViewById(R.id.num2);
result = findViewById(R.id.result);
add = findViewById(R.id.buttonAdd);
subtract = findViewById(R.id.buttonSubtract);
multiply = findViewById(R.id.buttonMultiply);
divide = findViewById(R.id.buttonDivide);
clear = findViewById(R.id.buttonClear);
```

在活动中，我们可以通过 findViewById() 方法获取布局文件中定义的元素，这里我们传入相应的 R.id.***, 来得到相应控件的实例( id 在 activity_main.xml 中使用 android:id 属性定义).

####设置运算按钮的监听事件：

以加法的处理为例子. 计算加法的描述如下：

```java
set 加法按钮监听 {
	while(按钮被点击){
		获取 EditText1 中的 number1；
		获取 EditText2 中的 number2；
		if(number1 != 0 && number2 != 0){
            result = number1 + number2;
            设置 TextView 的值为 result;
		}else{
            输出提示信息；
		}
	}
}
```

实现的代码：

```java
add.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        if(!num1.getText().toString().isEmpty() && !num2.getText().toString().isEmpty()){
            double n1 = Double.parseDouble(num1.getText().toString());
            double n2 = Double.parseDouble(num2.getText().toString());

            double res = n1 + n2;

            result.setText(String.valueOf(res));

        }else{
            Toast.makeText(v.getContext(), "Please Enter the numbers properly!", Toast.LENGTH_SHORT).show();
        }
    }
});
```

- **Button**.setOnClickListener(){}: 

  在按钮实例中，调用 setOnClickListener() 方法设置一个监听事件， 当按钮**被点击**时，就会执行监听器中的 **onClick()** 方法.

- **EditText**.getText().toString() ：

  从两个EditText中获取两个操作数。

- **Toast** (context(), content, Show_Length).show()：

  是 Android 系统提供的一个弹出式信息框，将短小的信息通知给用户并在一段时间后自动消失，且不占据屏幕空间。当用户输入不合规范时，我们让程序弹出一个 Toast，写明 “Please Enter the numbers properly!” 用于提示用户.

加减乘除四个方法的逻辑处理过程几乎完全相同，稍微更改一下即可.

####设置清除按钮的监听事件：

```javascript
clear.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {

        num1.setText(null);
        num2.setText(null);

        result.setText("Result");
    }
});
```

- **EditText**.setText( content ):

  设置 EditText 的内容，点击清除按钮即设两个 EditText 的内容为 null，初始化状态。

- **TextView**.setText( content ):

  设置 TextView 的内容，点击清除按钮即设内容为 “Result”

> MainActivity.java的完整代码：https://github.com/Ubilabs-NicholasCheng/SimpleCalculator/blob/6868921846cbd192a8430dc52ae036859dd12aff/app/src/main/java/com/example/nicholascheng/caculatar_test/MainActivity.java



## 美化界面

不得不说，这个原生的界面太简陋了。为了给用户一个良好的交互界面，我们可以引入外界依赖的 UI 库(这里用的是 bootstrap 与 material 风格的组件)，在 dependencies 中添加依赖：

```java
implementation 'com.beardedhen:androidbootstrap:2.3.2'
implementation 'com.rengwuxian.materialedittext:library:2.1.4'
```

![选区_142](/home/nicholascheng/Pictures/ScreenShots/选区_142.png)

这样，我们可以用这些库中有着丰富样式的组件啦。前端的设计非常有趣，经过简单布置布局后前端界面如下：

![选区_143](/home/nicholascheng/Pictures/ScreenShots/选区_143.png)

(虽然还是挺丑，但至少比简陋的原生组件要顺眼些…)

另外，不要忘记在 java 后端文件中更新你的组件类型(Button->BootstrapButton ;  TextView -> AwesomeTextView), 虽然编译器不会报错，但这两个类型并非完全兼容，直接运行使得控件无法被渲染，会导致app 闪退。

## 一些小技巧：

#### 选择最低兼容版本：

在File -> Project Structure -> Modules -> Flavors 中选择 Min Sdk Version，可以选择较低的 sdk 版本。

![选区_141](/home/nicholascheng/Pictures/ScreenShots/选区_141.png)

说明：Android 的版本控制比较复杂与繁琐，且 gradle 的向下兼容做的不是很好，所以高版本的 Android Studio 在运行低版本的项目时经常容易出现闪退等错误。解决版本问题的一个方法是选择最低的兼容版本，这样保证你的项目可以在大部分的设备平台上运行。



#### 查看报错：

在底部任务栏的logcat中，会动态显示项目构建与运行的所有状态信息。选择栏中选择Error可以过滤出所有的错误信息，便于开发者查错与调试。![选区_140](/home/nicholascheng/Pictures/ScreenShots/选区_140.png)



Android 开发的小技巧整理：https://github.com/tangqi92/Android-Tips



## 推荐阅读

多功能计算器实现：

https://www.zybuluo.com/stepbystep/note/67061

Android 图标库：

https://fontawesome.com/

http://www.iconfont.cn/

https://material.io/tools/icons/?search=divide&style=baseline

Android 主流 UI 开源库整理：

https://www.jianshu.com/p/47a4a7b99364

Github 代码浏览 chrome 插件：

http://blog.jobbole.com/112011/



