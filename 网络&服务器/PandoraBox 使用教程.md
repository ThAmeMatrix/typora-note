

# PandoraBox 使用教程

## 路由器刷入 Pandora 固件

要在路由器刷入 PandoraBox 固件，可以参考：

[https://github.com/ShiroCheng/typora-note/blob/master/%E7%BD%91%E7%BB%9C%26%E6%9C%8D%E5%8A%A1%E5%99%A8/PandoraBox%20%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.md](https://github.com/ShiroCheng/typora-note/blob/master/网络%26服务器/PandoraBox 使用教程.md)

该方法在 **2019.05.09** 测试成功.



## 路由器默认信息

SSID（Wifi 名）：PURE / PURE_5G

Wifi 密码： 509509509

网关：192.168.123.1

管理员账号：admin

管理员密码：admin



## 路由器网络配置

将刷好 Pandra 固件的路由器连接上电源，网线一端连接到路由器的 wan 口(橘黄色的端口)，另一端连接到寝室墙壁上的网口。

约20秒后路由器启动，在手机和电脑上就能搜索到路由器的Wi-Fi：**PURE** 和 **PURE_5G**，连接任意一个Wi-Fi，初始密码为 **509509509**

但连接后会发现，目前的 Wi-Fi 是无法访问互联网的，这是因为运营商对高校网络进行了限制，移动和电信在设备连接网线后还需要登陆其账号，才能访问互联网。因此在路由器里，我们同样需要设置访问运营商网络的用户信息。

在手机或电脑上连接 **PURE** 或 **PURE_5G** 任意一个Wi-Fi，然后打开浏览器，访问 http://192.168.123.1 进入路由器的配置管理页面：

![image-20190510161146041](https://ws1.sinaimg.cn/large/006tNc79ly1g2wafd5k8tj31ao0lijuy.jpg)

填写管理员用户名：admin，管理员密码：509509509 ，点击登陆。

登陆成功，进入路由器的配置界面：

![屏幕快照 2019-05-10 下午4.30.34](https://ws2.sinaimg.cn/large/006tNc79ly1g2wb64efmpj30zo0u0qf4.jpg)



选择外部网络

1. 移动网络的配置参考下图：

![屏幕快照 2019-05-10 下午9.55.30](https://ws1.sinaimg.cn/large/006tNc79ly1g2wkeg68brj30r81dodp9.jpg)

2. 电信网络的配置参考下图：

![image-20190510215409442](https://ws1.sinaimg.cn/large/006tNc79ly1g2wkbc9m6qj30r61dan78.jpg)

点击页面底部的按钮应用设置，大概10秒钟就会生效。



选择网络地图->地球的图标，可以查看外部网络的状态，显示已连接时表示 **Wi-Fi 可以正常上网**

![image-20190510165238600](https://ws4.sinaimg.cn/large/006tNc79ly1g2wblm086xj310m0u0qb6.jpg)



## 路由器系统管理

路由器的所有设置都由管理员账户设置，可以需要修改初始密码，保证自己的设备与数据安全

点击侧边栏的系统管理，修改管理员密码，新密码下次登陆的时候就会生效

![屏幕快照 2019-05-10 下午4.39.16](https://ws1.sinaimg.cn/large/006tNc79ly1g2wk46or7mj30u00xcqc9.jpg)



 Wi-Fi 名称和密码，更改后点击应用即可

注：2.4 G Hz 的 wifi 和 5 G Hz 的 wifi 初始密码为 509509509

![image-20190510164730601](https://ws4.sinaimg.cn/large/006tNc79ly1g2wbgan4ozj30u00vu7ef.jpg)



## 问题

若遇到外部网络连接不上等问题，可以在管理页面重启路由器。



## 速度测试

测试一下校园网移动和电信的网速

- 移动：

![屏幕快照 2019-05-10 下午4.00.36](https://ws1.sinaimg.cn/large/006tNc79ly1g2wkg5ezp7j31060u00z8.jpg)



- 电信：

![image-20190510220029618](https://ws4.sinaimg.cn/large/006tNc79ly1g2wki03gugj30zy0u044w.jpg)



移动的有线网相比电信慢一些，但和室友四人一天的使用情况看下来，观看视频和下载的速度还是比较稳定的，不考虑打网络游戏和看直播的情况移动网日用还是足够的。另外移动提供的是静态密码，路由器设置好一次后就可以一直联网，不像电信是动态密码每天要换一次……相比之下移动网还是方便的。