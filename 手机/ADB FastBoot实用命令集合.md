# ADB FastBoot实用命令集合

References： http://www.oneplusbbs.com/thread-3941475-1.html

Download Minimal ADB & Fastboot Tool for Android:

https://devsjournal.com/download-minimal-adb-fastboot-tool.html

## adb 相关

### adb 简介

借助adb工具， 我们可以管理设备或手机模拟器的状态， 还可以进行很多手机操作，比如安装软件，系统升级，运行shell命令等等， 其实简而言之， adb就是连接Android手机与PC端的桥梁，可以让用户在电脑上对手机进行全面的操作

（1）快速更新设备或手机模拟器中的代码，如应用或Android系统升级；
（2）在设备上运行Shell命令；
（3）管理设备或手机模拟器上的预定端口；
（4）在设备或手机模拟器上复制或粘贴文件。



### adb 常用命令

**刷机相关命令**

```shell
# 重启到 Recovery 模式
adb reboot recovery

# 从 Recovery 重启到 Android
adb reboot

# 重启到 Fastboot 模式
adb reboot bootloader

# 禁用系统更新
adb shell pm disable-user com.oneplus.opbackup

# 启用
adb shell pm enable com.oneplus.opbackup

# 安装一个apk
adb uninstall <package> // 如：adb uninstall com.stormzhang.demo

# 卸载 app 但保留数据和缓存文件
adb uninstall -k com.stormzhang.demo

# 复制设备里的文件到电脑，如 adb pull /sdcard/test.txt D:\
adb pull <设备里的文件路径> [电脑上的目录]

# 复制电脑的文件到设备, 如 adb push D:\test.txt /sdcard/
adb push <电脑上的文件路径> <设备里的目录>

# 查看手机信息
adb shell getprop ro.product.model

# 获取IMEI
adb shell
su
service call iphonesubinfo 1

##### 实用功能 #####
# 去除无线X的标志
adb shell settings put global captive_portal_detection_enabled 0

# Https的信号检测地址
adb shell settings put global captive_portal_https_url https://www.google.cn/generate_204

# 模拟按键
adb shell input keyevent <Value>
 # Value    含义
  # 26	   电源键
  # 82	   菜单键
  # 64	   打开浏览器
  # 3	   HOME 键
  # 4	   返回键
  # 176	   打开系统设置
```



## Fastboo**相关**

### Fastboot 简介

**快速启动**（英语：Fastboot）是一个诊断协议，主要用于由一台计算机通过[USB](https://zh.wikipedia.org/wiki/USB)连接[Android](https://zh.wikipedia.org/wiki/Android)[智能手机](https://zh.wikipedia.org/wiki/%E6%99%BA%E8%83%BD%E6%89%8B%E6%9C%BA)，修改其[闪存](https://zh.wikipedia.org/wiki/%E9%97%AA%E5%AD%98)[文件系统](https://zh.wikipedia.org/wiki/%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)。它是[Android Debug Bridge](https://zh.wikipedia.org/w/index.php?title=Android_Debug_Bridge&action=edit&redlink=1)库的一部分。利用快速启动协议要求该设备在引导装载程序或第二程序加载器模式下启动，这种情况下只进行最基本的硬件初始化。当设备本身的执行协议通过后，它会接受任何经由USB接口发送过来的命令行指令。

### Fastboot 常用命令

```shell
# 重启系统
fastboot reboot 

# 重启到 bootloader
fastboot reboot-bootloader

# 获取手机端变量信息
fastboot getvar version:version-bootloader:version-baseband:product:serialno:secure
# version	  客户端支持的fastboot协议版本
# version-bootloader	  Bootloader的版本号
# version-baseband	基带版本
# product	产品名称
# serialno	产品序列号
# secure	返回yes 表示在刷机时需要获取签名

# fastboot erase(清除分区)
# 清除boot分区
fastboot erase boot

# 清除System分区
fastboot erase system

# 清除Data分区
fastboot erase data

# 清除cache分区
fastboot erase cache

# 上面的命令也可以简化成一条命令，也就是俗称的四清
fastboot erase system -w

# fastboot flash(烧写指定分区)
fastboot flash {partition} {*.img}

# flash recovery img
fastboot flash recovery recovery.img
```

