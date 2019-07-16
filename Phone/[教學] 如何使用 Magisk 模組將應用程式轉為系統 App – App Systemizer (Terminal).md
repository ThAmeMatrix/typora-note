# [教學] 如何使用 Magisk 模組將應用程式轉為系統 App – App Systemizer (Terminal)

2018.09.17 / 分類: [刷機相關](http://izaka.tw/category/android-phone/flash/), [安卓系統](http://izaka.tw/category/android-phone/) / [0 則回應](http://izaka.tw/magisk-app_systemizer-guide/#respond)

標籤: [Magisk](http://izaka.tw/tag/magisk/)

来自：http://izaka.tw/magisk-app_systemizer-guide/

在 Android 8.0 Oreo 導入 [Privileged Permission Whitelisting](https://source.android.com/devices/tech/config/perms-whitelist) 之後，要將 App 變成具備系統權限 App (/system/priv-app)，最容易上手的方式就是使用基於 stangri 所釋出的 [App Systemizer](https://forum.xda-developers.com/apps/magisk/module-app-systemizer-t3477512) 啟發，由 veez21 所釋出的 [Terminal] App Systemizer v14.2，本文著重在使用需求以及操作流程，對於 Magisk 不熟悉的朋友，可以參考 ｢[[教學\] Magisk v17.1 – The Universal Systemless Interface 簡易安裝流程 (含 Root、Xposed)](http://izaka.tw/android-magisk-installation-guide/)｣一文。



## 安裝 App Systemizer (Terminal Emulator) 模組

![img](https://i1.wp.com/img.izaka.tw/20180916204545_67.jpg?resize=1024%2C768)
 開啟 Magisk Manager 點擊｢模組｣，搜尋 Systemizer

![img](https://i1.wp.com/img.izaka.tw/20180916204549_99.jpg?resize=1024%2C768)
 點擊下載圖示並安裝，並於安裝完畢後重啟手機

## 安裝終端模擬器 Termux

![img](https://i2.wp.com/img.izaka.tw/20180917003012_85.jpg?resize=1024%2C768)
 一般來說，提到安卓的終端模擬，大家映入腦海中的非 [Terminal Emulator for Android](https://play.google.com/store/apps/details?id=jackpal.androidterm) 莫屬，不過實際測試在列表清單的部分會有段行對照的問題，這邊改選用 Termux，安裝連結可於 Play 商店搜尋或點擊下方圖示
[![img](https://i0.wp.com/img.izaka.tw/20180907132202_92.png?resize=160%2C62)](https://play.google.com/store/apps/details?id=com.termux)

## App Systemizer (Terminal Emulator) 操作流程

![img](https://i1.wp.com/img.izaka.tw/20180917003017_7.jpg?resize=1024%2C768)
 首次輸入 **su** 要取得權限時，會跳出超級用戶請求，此時按下允許

![img](https://i2.wp.com/img.izaka.tw/20180917144855_85.jpg?resize=1024%2C768)
 在輸入 su 取得權限後，鍵入 systemize 後就可看到 App Systemizer (Terminal Emulator) 操作選單，這時鍵入 1 並執行

![img](https://i0.wp.com/img.izaka.tw/20180917144857_98.jpg?resize=1024%2C768)
 在存取一陣子後，即可看到目前手機的 App 與對應編號

![img](https://i0.wp.com/img.izaka.tw/20180917144859_86.jpg?resize=1024%2C768)

1. 可輸入多組要 Systemize 的 App 編號，並用空白隔開
2. 選擇將 App 移至 /system/priv-app
3. 看到 Granting Permissions App – Done 即代表成功

## 還原並去除系統 App 權限

![img](https://i0.wp.com/img.izaka.tw/20180917144901_14.jpg?resize=1024%2C768)
 重新進到 App Systemizer 選單，輸入 4 執行 Revert Systemized Apps 後，就會看到目前手機系統中曾經 Systemized 過的 App，同樣輸入數字編號後即可移除，並於手機重開機後生效