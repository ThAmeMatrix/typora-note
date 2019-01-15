# 小米 8 刷 Havoc-OS 过程记录 (从零开始的刷机入门)

### 时间：2018.12.30

于鄙人实在忍受不了 miui 设计清奇的界面风格(国产仿IOS圆角和配色)，以及和谷歌服务整合得非常糟糕的小米系统服务，决定尝尝原生。

在下是刷机小白，第一次用twrp操作，于是把过程也记录一下，错漏的地方麻烦酷友指正。

## 简介
Havoc-OS 目前是 xda 论坛中非常活跃的 Rom, 优点在于轻量，快速，高度的可定制化，并支持人脸解锁与 NFC 等诸多特性，在12月的更新后系统界面也有了比较大的改观。

该 rom 的特性以及刷机的方法作者已经在帖子中很明确地阐述：
https://forum.xda-developers.com/mi-8/development/prepare-t3841106 ，本文仅记录额外的流程与坑点。


## 环境

MI 8 普通版（MIUI国内版） * 1 ,  数据线 * 1， Windows 电脑 * 1


## 1. 解锁 BootLoader

解锁 BootLoader，参考官方流程：http://www.miui.com/unlock/index.html

## 2. 刷入 Twrp

Mi 8 Twrp 下载： http://www.miui.com/thread-15306132-1-1.html 
这位大佬修改的 TWRP 做了汉化并支持 Android 9 的 data 分区解密， 并附带了一键刷入脚本与 adb 工具。注意：请务必使用 3.2.3 及以上版本保证 data 分区解密，防止刷国际版后无限在 TWRP 界面重启。

## 3. 刷入 Havoc

作者介绍刷入 Havoc 的方法（版本 Android 9）：

```shell
Backup your data !

1. Flash MIUI PIE 8.12.20 global Rom DON'T REBOOT
2. Flash Disable_Dm-Verity_ForceEncrypt Here DON'T REBOOT
3. Wipe Data,System,Cache Your Data will be lost
4. Flash HavocRom + Gapps
5. Reboot and Enjoy
```

翻译成中文，即需要的东西有：

1. MIUI 国际版最新开发版 ROM，下载地址：http://en.miui.com/thread-4970588-1-1.html
2. Disable_Dm-Verity_ForceEncrypt，下载地址：https://drive.google.com/open?id=1CqEOK1mOkxeVbj2S3TBz10IutBl4FZBI
3. Havoc-OS ROM，下载地址：https://sourceforge.net/projects/havoc-os/files/dipper/
4. Gapps，下载地址：https://opengapps.org/  
需要科学上网，网盘：https://pan.baidu.com/s/1PWZjbVZTYuLVcj6L2IhNKw   提取码：uvrr 
5. Magisk，可选，最新 TWRP 中也自带了 Magsisk，可以直接在 TWRP 中完成 ROOT. 下载地址：https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445

在电脑上下载好上述包后，将 MIUI 国际版 ROM 拷贝到手机内置存储中，然后，长按电源键和音量加号键，进入 TWRP 界面，执行以下操作：

1. 点击 wipe 选项， 选高级清除， **清除 System，Data，Cache分区**，再格式化 Data，格式化 Data 的步骤会清理手机内部存储，导致手机内部没有刷机包。(不要重启)
2. 使用 adb 命令，将其他的几个刷机包都拷贝到手机内置存储中，命令如下：
```shell
$ adb push <Your-PackageName> /sdcard/.
```
3. 安装 Disable_Dm-Verity_ForceEncrypt(不要重启)
4. 再次点击 wipe 选项**清除 System，Data，Cache分区**
5. 安装 HavocRom，Gapps

重启系统，进入登录画面

## 5. 跳过开机验证
由于原生系统首次开机时需要向 Google 发送本机验证消息，但此时并没有科学辅助工具。解决方案：https://vitan.me/2018/05/04/Win10-Share-WiFI/

之后就可以开始享受了… 

(本人因为是电信卡，一开始有点虚…没想到不需要配置， 手机 WIFI , Mobile Data 和 HostSpot 就都能完美使用，非常省心)

## 6. 开启 Google Feed (可选)
参考酷安大佬的教程：https://www.coolapk.com/feed/8901701

## 心得体会
最后的一点心得…第一次接触 TWRP，感觉这东西简直神器啊，手机没系统了一点不慌，只要能进 fastboot 刷个 twrp 后分分钟帮你把系统重刷回来…希望自己能不那么快回 miui 把…

## 7. 修复 TWRP 触屏失灵
2019.1.15更新：
- 先前刷了havoc的rom, 今天进了一次twrp后不管再进rom和twrp触摸屏幕都失灵了。
我尝试在fastboot中格式化了system data cache分区，并线刷了miui10 ，但重启后触屏仍然不能工作，可能是什么原因？
希望不是成砖了…现在害怕的一匹，求助各位大佬[受虐滑稽][受虐滑稽]

解决方案：重刷 miui 最新稳定版
