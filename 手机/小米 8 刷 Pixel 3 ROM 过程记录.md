# 小米 8 刷 Pixel 3 ROM 过程记录
Pixel 3 ROM 官网介绍：https://forum.xda-developers.com/mi-8/development/rom-google-pixelrom-v3-0sep-mi-8-t3843356

## 使用体验
鄙人第二次刷机…先简单说说该 ROM 的使用体验。

与 Mi8 另一个活跃的 ROM **Havoc** 相对比，鄙人认为 **Pixel 3 ROM** 有以下亮点：
- 更流畅的动画，动画与渐变过渡很自然，不像 Havoc 会出现动画掉帧
- 更鲜艳的色彩、精细的像素显示，不像 Havoc 偶尔有锯齿
- 更好的应用兼容性，基本不会出现应用闪退
- 更接近 Google Pixel 的原生体验(废话)，ROM 自带 Google 全家桶
- 机型会被识别为 Google Pixel 3 (好评!)， 发动态自带谷歌亲儿子小尾巴
- 续航稍好于 Havoc，不用绿守黑域的话亮屏在 6 小时左右
- 电信卡可以放心，4G LTE、电话、热点均可正常使用

劣势也明显：
- 缺少 Havoc 的自定义功能 (废话，作者都说了是 Stock Androidroid 哪来自定义），如没有系统全面屏手势、没有各种骚气的颜色自定义、系统不自带换字体功能等等

个人认为总体来讲 Pixel 3 ROM 的使用体验优于 Havoc，下面是刷入过程记录:

## 刷入过程
### 解锁
- 解锁bootloader， 刷入 twrp，请参考：
https://bbs.letitfly.me/d/743

### 下载
- MIUI Global Firmware 8.11.x (MIUI 官方固件): 
https://xiaomifirmwareupdater.com/#weekly
- DM Verity & Force Encrypt disabler(强制加密禁用程序): https://drive.google.com/file/d/1CqEOK1mOkxeVbj2S3TBz10IutBl4FZBI/view?usp=drive_open
- Pixel 3 ROM (2019-01-09): 
https://drive.google.com/uc?id=1smRhV3H51GsYJ9_XkkEjJyw9WuNOohlu&export=download

### 刷入
0. 重启到 twrp
1. 清除 data, cache, system, 并格式化 data 分区
2. 使用 adb 命令，将之前下载的所有刷机包都拷贝到手机内置存储中：
    ```shell
    # 复制电脑的文件到设备, 如 adb push D:\test.txt /sdcard/
    $ adb push <电脑上的文件路径> <设备里的目录>
    ```
    更多的 adb 与 fastboot 实用命令，可参考：
    http://www.oneplusbbs.com/thread-3941475-1.html

    如果不使用上述的 adb 指令，可以尝试 otg u盘或转接线直接拷贝文件
3. 刷入 MIUI 8.11.x Firmware
4. 清除 system, data, chache
5. 刷入 Pixel 3 ROM
6. 刷入 DM Verity & Force Encrypt
7. 重启并享受吧

如果刷机过程中你遇到了问题，可参考鄙人的另一篇详细刷机记录：https://github.com/ShiroCheng/typora-note/blob/master/%E6%89%8B%E6%9C%BA/%E5%B0%8F%E7%B1%B3%208%20%E5%88%B7%20Havoc%20%E8%BF%87%E7%A8%8B%E8%AE%B0%E5%BD%95.md

## 已知 bug
红外解锁，长焦相机，滑动后置指纹区域查看消息，以上功能无法使用

两次按电源键快捷启动相机后，拍出的照片不会保存到相册