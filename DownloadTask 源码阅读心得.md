# DownloadTask 源码阅读心得

## 程序结构

DownloadTask ：工具类， 使用 AsyncTask 发送 HTTP 请求从网络异步获取数据。

DownloadCallback：接口，提供了下载任务进行到不同状态时的信息。

MyApplication ：application， 在项目创建 Application 时初始化WebSQL工具供进行数据库调试。

NetworkFragment：运行AsyncTask从网络获取数据。

MainActivity: 主活动，实现了 DownloadCallback 方法。



## Callback 的用法说明

