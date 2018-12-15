# 阿里云ECS服务器环境搭建（4） —— ubuntu 16.04下 mongodb无法从公网进行远程连接

原文：https://blog.csdn.net/zwq912318834/article/details/80570397



### 3.1. 修改mongodb的配置文件 /etc/mongod.conf，绑定到任何ip上

- 启动mongodb，使用如下命令查看，端口绑定情况

```
# 启动mongodb
service mongod start
# 查看网络端口情况，发现mongodb服务绑定中本地ip上： 127.0.0.1：27017
netstat -tunlp1234
```

![这里写图片描述](https://img-blog.csdn.net/20180605103741298?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3cTkxMjMxODgzNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
# 打开mongodb配置文件
gedit /etc/mongod.conf

# 修改配置文件：绑定到任何IP上
bind_ip = 0.0.0.0
# 关闭认证
auth = false1234567
```

![这里写图片描述](https://img-blog.csdn.net/20180605103159461?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3cTkxMjMxODgzNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
# 重启mongodb
service mongod stop
service mongod start
# 查看网络端口情况，发现mongodb服务已经绑定在任意ip上了： 0.0.0.0：27017
netstat -tunlp12345
```

![这里写图片描述](https://img-blog.csdn.net/20180605103951326?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3cTkxMjMxODgzNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



### 之后，修改ECS 云服务器安全组规则，对外开放 27017 端口

- 完成这一步之后，就可以通过终端或者NoSQLBooster这样的可视化工具进行连接和操作了。

 4. 结果

- 完成上一步之后，就可以在**本地windows端**，通过下面两种方式进行连接了。 
  ![这里写图片描述](https://img-blog.csdn.net/20180605105521330?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p3cTkxMjMxODgzNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)