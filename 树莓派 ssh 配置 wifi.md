# 无屏幕和键盘配置树莓派WiFi和SSH

[2017年9月13日](http://shumeipai.nxez.com/2017/09) [Spoony](http://shumeipai.nxez.com/author/admin) [未分类](http://shumeipai.nxez.com/category/uncategorized) [12](http://shumeipai.nxez.com/2017/09/13/raspberry-pi-network-configuration-before-boot.html#mh-comments)

![树莓派 Zero W](http://shumeipai.nxez.com/wp-content/uploads/2017/03/20170301115718410-0.jpg)
不算是什么新功能了，在树莓派3B发布后不久，树莓派官方 Raspbian 系统久加入了允许在开机前对 WiFi 网络进行配置的机制。

#### 一、WiFi 网络配置

用户可以在未启动树莓派的状态下单独修改 `/boot/wpa_supplicant.conf` 文件配置 WiFi 的 SSID 和密码，这样树莓派启动后会自行读取 wpa_supplicant.conf 配置文件连接 WiFi 设备。

操作方法简单：将刷好 Raspbian 系统的 SD 卡用电脑读取。在 boot 分区，也就是树莓派的 `/boot` 目录下新建 wpa_supplicant.conf 文件，按照下面的参考格式填入内容并保存 wpa_supplicant.conf 文件。

```
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
 
network={
ssid="WiFi-A"
psk="12345678"
key_mgmt=WPA-PSK
priority=1
}
 
network={
ssid="WiFi-B"
psk="12345678"
key_mgmt=WPA-PSK
priority=2
scan_ssid=1
}
```

说明以及不同安全性的 WiFi 配置示例：
#ssid:网络的ssid
#psk:密码
#priority:连接优先级，数字越大优先级越高（不可以是负数）
#scan_ssid:连接隐藏WiFi时需要指定该值为1

如果你的 WiFi 没有密码

```
network={
ssid="你的无线网络名称（ssid）"
key_mgmt=NONE
}
```


如果你的 WiFi 使用WEP加密

```
network={
ssid="你的无线网络名称（ssid）"
key_mgmt=NONE
wep_key0="你的wifi密码"
}
```

如果你的 WiFi 使用WPA/WPA2加密

```
network={
ssid="你的无线网络名称（ssid）"
key_mgmt=WPA-PSK
psk="你的wifi密码"
}
```


如果你不清楚 WiFi 的加密模式，可以在安卓手机上用 root explorer 打开 `/data/misc/wifi/wpa/wpa_supplicant.conf`，查看 WiFi 的信息。

#### 二、开启 SSH 服务

如果通过 ssh 连接树莓派出现 Access denied 这个提示则说明 ssh 服务没有开启。要手动开启的话，和 WiFi 配置相似，同样在 boot 分区新建一个文件，空白的即可，文件命名为 ssh。注意要小写且不要有任何扩展名。
树莓派在启动之后会在检测到这个文件之后自动启用 ssh 服务。随后即可通过登录路由器找到树莓派的 IP 地址，通过 ssh 连接到树莓派了。（[有关开启 SSH 服务的详细方法](http://shumeipai.nxez.com/2017/02/27/raspbian-ssh-connection-refused.html)）

如果需要远程桌面方式操作树莓派，可以通过 ssh 安装 xrdp，[再用 Windows 的远程桌面客户端连接到树莓派](http://shumeipai.nxez.com/2013/10/06/windows-remote-desktop-connection-raspberry-pi.html)。

这个小技巧对于没有有线网卡、没有标准 USB 接口来直连键鼠，但集成了 WiFi 的[树莓派 Zero W](https://item.taobao.com/item.htm?id=557980870076) 尤其实用。