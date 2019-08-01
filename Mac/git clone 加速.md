# git clone 加速

## 起因

为了修改自己博客的文件夹命名，就需要将github上的仓库下载到本地，使用git clone时，速度慢到无法接受，就Google了一番git clone加速的办法，记录下来，以便后续查阅。

github速度慢是因为DNS被污染了，原因你懂的，对于解决github的问题，使用代理可能是最好的办法了

## 使用代理

使用这个方法的前提，是你有一个天梯，这个天梯能够让你正常访问Google，天梯怎么搭，这里就不赘述了，我们这里假设你已经有天梯可用，可以设置git通过代理进行访问；

起初我使用了v2ray的全局模式，执行git clone后，发现速度并没有任何变化，Google后发现，git命令并不会直接走全局代理，需要通过git config配置，看完所有命令再操作；

```
# 千万别急，刚开始而已
# socks5协议，1080端口修改成自己的本地代理端口
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080

# http协议，1081端口修改成自己的本地代理端口
git config --global http.proxy http://127.0.0.1:1081
git config --global https.proxy https://127.0.0.1:1081
复制代码
```

以上的配置会导致所有git命令都走代理，但是如果你混合使用了国内的git仓库，甚至是局域网内部的git仓库，这就会把原来速度快的改成更慢的了；

下面是仅仅针对github进行配置，让github走本地代理，其他的保持不变；

```
# socks5协议，1080端口修改成自己的本地代理端口
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080
git config --global https.https://github.com.proxy socks5://127.0.0.1:1080

# http协议，1081端口修改成自己的本地代理端口
git config --global http.https://github.com.proxy https://127.0.0.1:1081
git config --global https.https://github.com.proxy https://127.0.0.1:1081
复制代码
```

其他几个相关命令：

```
# 查看所有配置
git config -l
# reset 代理设置
git config --global --unset http.proxy
git config --global --unset https.proxy
git config --global --unset http.https://github.com.proxy
git config --global --unset https.https://github.com.proxy
复制代码
```

看下使用了本地代理前后，速度的差距：



![img](https://user-gold-cdn.xitu.io/2019/6/10/16b41bff33b76332?imageView2/0/w/1280/h/960/ignore-error/1)



## Reference

1. [给 github clone 加速](https://link.juejin.im/?target=http%3A%2F%2Fstruggleblog.com%2F2018%2F07%2F13%2Faccelerate_github_clone%2F)
2. [github代码clone加速](https://link.juejin.im/?target=http%3A%2F%2Fnullpointer.pw%2Fgithub%E4%BB%A3%E7%A0%81clone%E5%8A%A0%E9%80%9F.html)

第2点的这个方法，没试过，不知道可不可行，感兴趣的朋友可以试试看。

[原文链接](https://link.juejin.im/?target=https%3A%2F%2Fc1rew.github.io%2F2019%2F05%2F19%2Fgit-clone-%E5%8A%A0%E9%80%9F%2F)

关注下面的标签，发现更多相似文章

