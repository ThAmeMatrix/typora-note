# Android Studio 快速上手

[TOC]

## 0. Android 应用基础知识

Android 应用采用 Java 编程语言编写。Android SDK 工具将您的代码 — 连同任何数据和资源文件 — 编译到一个 APK：*Android 软件包*，即带有 `.apk` 后缀的存档文件中。一个 APK 文件包含 Android 应用的所有内容，它是基于 Android 系统的设备用来安装应用的文件。

 安装到设备后，每个 Android 应用都运行在自己的安全沙箱内：

- Android 操作系统是一种多用户 Linux 系统，其中的每个应用都是一个不同的用户；
- 默认情况下，系统会为每个应用分配一个唯一的 Linux 用户 ID（该 ID 仅由系统使用，应用并不知晓）。系统为应用中的所有文件设置权限，使得只有分配给该应用的用户 ID 才能访问这些文件；
- 每个进程都具有自己的虚拟机 (VM)，因此应用代码是在与其他应用隔离的环境中运行；
- 默认情况下，每个应用都在其自己的 Linux 进程内运行。Android 会在需要执行任何应用组件时启动该进程，然后在不再需要该进程或系统必须为其他应用恢复内存时关闭该进程。



## 1. Android Studio 简介

Android Studio 是基于IntelliJ IDEA开发的 Android 集成开发环境, 于2013 Google I/O大会发布。

### Android Studio 优势

- 基于Gradle构建支持
- 针对Android的专属构建与快速修复,开发更智能
- 支持ProGuard和应用签名
- 丰富的布局编辑器
- 完善的插件系统
- 完美的版本控制支持



### 学习资料

1. 官方文档：https://developer.android.com/guide/?hl=zh-cn

注: ~~(Google 提供的 android developer 官方文档, 介绍得详细且全, 但术语较多阅读有一定难度)~~



2. Android 开发文档·中文版：http://hukai.me/android-training-course-in-chinese/index.html	

注: ~~(是官方文档的中文翻译版, 大概一年半没有更新了, 但对android项目的介绍很清晰明了, 推荐阅读)~~



3. 《第一行代码：Android（第2版）》作者: 郭霖

注: ~~(被广大Android 开发者誉为“Android 学习第一书”。本书内容通俗易懂，由浅入深，既是Android 初学者的入门必备，也是Android 开发者的进阶首选, 墙裂推荐)~~



 

## 2. Android Studio 安装配置

#### 1. JDK安装配置

JDK下载:  http://www.oracle.com/technetwork/java/javase/downloads/

安装JDK, 配置环境变量

- JAVA_HOME
- PATH+=;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin
- CLASSPATH=.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar

#### 2. Android Studio 下载安装

http://www.android-studio.org/

根据系统按下载对应安装包, 按照向导安装 Android Studio



## 3. 创建 Android 项目

本环节将介绍如何使用 Android Studio 创建一个新的 HelloWorld 项目, 并说明该项目中的一些文件功能。

1. 在 **Welcome to Android Studio** 窗口中，点击 **Start a new Android Studio project**。

![选区_171](/home/nicholascheng/Pictures/ScreenShots/选区_171.png)

或者如果您已打开项目，请选择 **File > New Project**。

2. 在 **New Project** 屏幕中，输入以下值：

   - **Application Name**：“HelloWorld”

   您可能需要更改项目位置，但无需更改其他选项。

3. 点击 **Next**。

4. 在 **Target Android Devices** 屏幕中，保留默认值并点击 **Next**。

5. 在 **Add an Activity to Mobile** 屏幕中，选择 **Empty Activity**，然后点击 **Next**。

6. 在 **Configure Activity** 屏幕中，保留默认值并点击 **Finish**。

经过一些处理后，Android Studio 将打开 IDE。![选区_174](/home/nicholascheng/Pictures/ScreenShots/选区_174.png)



**下面我们了解一下 Android Studio 的界面, 以及重要的文件:**

#### 1.界面元素

![选区_163](/home/nicholascheng/Pictures/ScreenShots/选区_163.png)

#### 2. 工具栏

![2018-07-10 07-36-24屏幕截图](/home/nicholascheng/Pictures/ScreenShots/2018-07-10 07-36-24屏幕截图.png)

#### 3. 项目 app的基本结构

(带*号为重要的文件, 未标注注释的文件为系统自动生成,一般无需更改)

![选区_168](/home/nicholascheng/Pictures/ScreenShots/选区_168.png)![选区_167](/home/nicholascheng/Pictures/ScreenShots/选区_167.png)

#### 4. 编译配置文件 build.gradle(module)

```json
apply plugin: 'com.android.application'

android {
    //制定编译用的SDK版本号,如27表示用Android 8.1编译
    compileSdkVersion 27
    defaultConfig {
    	//指定该模块的应用编号,即App的包名.该参数为自动生成,无需更改
        applicationId "com.example.nicholascheng.helloworld"
    	//指定App适合运行的最低SDK版本号,如16表示至少要在Android 4.1上运行
        minSdkVersion 16
    	//指定目标设备的SDK版本号,即该App最希望在哪个版本的Android上运行
        targetSdkVersion 27
    	//指定App的应用版本号
        versionCode 1
    	//指定App的应用版本名称
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    //指定引用jar包的路径
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    //指定编译Android的高版本支持库,如AppCompatActivity必须指定编译appcompat-v7库
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.2'
    implementation 'com.android.support:design:27.1.1'
    //指定单元测试编译用的junit版本号
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}
```



## 4. 运行 Android 项目

### 在真机上运行:

环境准备，将你的设备调整为如下状态：

1. 通过USB数据线连接手机到你的电脑上。如果你的电脑系统是Windows，你可能需要为你的电脑安装一个合适的USB驱动。驱动安装帮助，请参见 [OEM USB Drivers](https://developer.android.com/studio/run/oem-usb.html) 文档。
2. 能够使用 **USB调试** 你的手机，打开 **设置 > 开发者选项**

> **提示**：在Android4.2以及更高的版本上， **开发者选项** 默认是隐藏的。打开 **设置 > 关于手机** ，连续点击 **版本号** 七次。返回到设置界面去找 **开发者选项**。



在Android Studio上运行App的方式如下：

1. 在Android Studio上选中项目，然后点击在工具栏上的 **Run**。
2. 在 **Select Deployment Target** 的对话框中，选择设备并点击 **OK**。

Android开始安装App在设备上，并会启动它。



### 在虚拟机上运行:

如果你之前没有运行App在模拟器上，你需要先创建一个 [Android模拟器](https://developer.android.com/tools/devices/index.html) （AVD）。配置一个类型如Android手机、平板、穿戴、TV在你的Android模拟器上。

创建Android模拟器的方式如下：

1. 打开Android模拟器管理窗口，**工具 》 Android 》 Android模拟器管理**，或者点击工具栏上的快捷图标。
2. 在 **你的虚拟设备** 页面中，点击 **创建虚拟设备** 。
3. 在 **选择硬件** 页面中，选择一款手机，如Nexus 6，接着点击 **Next**。
4. 在 **系统镜像** 页面中，为模拟器选择一个想要的系统，如Nougat, 接着点击 **Next**。

(如果你选中的那个系统镜像没有安装，请点击“下载”链接进行安装)

5. 检查配置设置（对于第一个AVD，保持原样的设置），点击 **Finish**。



在Android Studio上运行App的方式如下：

1. 在 **Android Studio** 上选中项目，然后点击 **Run** 在工具栏上。
2. 在**Select Deployment Target**的对话框中，选择设备并点击 **OK**。

让这个模拟器启动你需要等待一会。之后你需要解锁屏幕。当你解锁后，“HelloWorld”就展现在模拟器上了。



## 5. Android Studio 调试

1. 设置断点

![选区_177](/home/nicholascheng/Pictures/ScreenShots/选区_177.png)

2. 菜单Run->Debug

3. 显示调试器窗口

   ![选区_176](/home/nicholascheng/Pictures/ScreenShots/选区_176.png)

4. 使用调试工具分析代码



## 6. Android Studio 导入工程

1. 欢迎界面: Check out Project from Version Control
2. 或 File-> New-> Project From Version Control-> Github
3. 输入github地址, 下载目录
4. 点击Clone

![选区_178](/home/nicholascheng/Pictures/ScreenShots/选区_178.png)



## 7. Android Studio 版本控制

1. git的下载安装, 链接：<http://git-scm.com/download/>

2. 将git工具和studio关联

      ![选区_190](/home/nicholascheng/Pictures/ScreenShots/选区_190.png)

3. 将Android Studio 与 Github 账号关联

      ![选区_192](/home/nicholascheng/Pictures/ScreenShots/选区_192.png)

4. 上传工程到 Github:

   打开你要上传的工程，顶部菜单选择VCS->Import into Version Control->Share Project on GitHub

   > 注: 如果你是第一次提交该项目会出现对话框，提示你这是一个新的存储库（repo），可以自定义repo的名字，和添加描述。

5. 输入你的Master password点击ok，若提交成功Android Studio右上角会提示相关信息

![选区_194](/home/nicholascheng/Pictures/ScreenShots/选区_194.png)