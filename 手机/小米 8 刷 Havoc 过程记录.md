# 小米 8 刷 Havoc-OS 过程记录

由于鄙人实在忍受不了 miui 设计清奇的界面风格(国产仿IOS圆角和配色)，以及和谷歌服务整合得非常糟糕的小米系统服务，决定尝尝原生。鄙人是刷机小白，刚开始用twrp不小心四清了手机(System, Cache, Data, Internal Stroage)差点把手机变砖了，于是把过程也记录一下，错漏的地方麻烦酷友指正。

## 环境：

MI 8 普通版 * 1

Windows 电脑 * 1

## 解锁 BootLoader

解锁 BootLoader，参考官方教程：http://www.miui.com/unlock/index.html

## 刷入 Twrp

刷入 Twrp，可参考：http://www.miui.com/thread-12263814-1-1.html，介绍的过程很详细。具体操作也比较简单，使用 adb 工具将 twrp 的 image 文件写入手机即可。注意刷入 twrp 后要刷入 Magisck 或 Supersu，防止 twrp 被官方recovery 替代。如果被替代就要进 fastboot 重刷一次。

## 刷入 Havoc

Havoc-OS 论坛介绍：https://forum.xda-developers.com/mi-8/development/prepare-t3841106

作者介绍刷入 Havoc 的方法（版本 Android Pie）：

```shell
Backup your data !

1. Flash MIUI PIE 8.12.20 global Rom DON'T REBOOT
2. Flash Disable_Dm-Verity_ForceEncrypt Here DON'T REBOOT
3. Wipe Data,System,Cache Your Data will be lost
4. Flash HavocRom + Gapps
5. Reboot and Enjoy
```

翻译成中文，也就是说要准备的东西有：

1. MIUI 国际版最新开发版 ROM，下载地址：http://en.miui.com/thread-4970588-1-1.html
2. Disable_Dm-Verity_ForceEncrypt，下载地址：https://drive.google.com/open?id=1CqEOK1mOkxeVbj2S3TBz10IutBl4FZBI
3. Havoc-OS ROM，下载地址：https://sourceforge.net/projects/havoc-os/files/dipper/
4. Gapps，下载地址：https://opengapps.org/  
需要科学上网，网盘：https://pan.baidu.com/s/1PWZjbVZTYuLVcj6L2IhNKw   提取码：uvrr 
5. Magisk，可选，最新 TWRP 中也自带了 Magsisk，可以直接在 TWRP 中完成 ROOT. 下载地址：https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445

然后，进入 TWRP后：

1. 点击 wipe 选项**清除 System，Data，Cache分区**，再安装 MIUI 国际版 Rom （我的mi8开始是国内的开发版，没想太多直接刷入了 MIUI 国际版 ROM ，结果按步骤操作完重启后发现卡死在开机界面，只好重刷…）
2. 安装 Disable_Dm-Verity_ForceEncrypt
3. 再次点击 wipe 选项**清除 System，Data，Cache分区
4. 安装 HavocRom，Gapps

重启系统，就可以享受了… 
(一开始因为是电信卡还有点虚…没想到不需要配置 WIFI , Mobile Data 和 HostSpot 就都能完美使用，瞬间舒服了！)

## 开启 Google Feed
请参考这位大佬的教程：https://www.coolapk.com/feed/8901701

## 爬坑

由于操作失误，我在 TWRP 中清除了内部存储和 System 分区后，既没有系统也没有刷机包，TWRP 中的 MTP 也无法正常传输电脑上的刷机包，陷入非常的尴尬的情况…我折腾了好久，最后在油管中找到了解决方案，原来 **adb 指令**可以直接拷贝(命令行大法好)而且速度很快，指令如下：

```shell
$ adb push <Your-PackageName> /sdcard/.
```

这条指令将文件拷贝到 Android Phone 的 /sdcard 目录下。

最后的一点心得…第一次接触 TWRP，感觉这东西简直神器啊，手机没系统了一点不慌，只要能进 fastboot 刷个 twrp 后分分钟帮你把系统重刷回来…希望自己能不那么快回 miui 把…

