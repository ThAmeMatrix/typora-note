# 使用OneDrive备份同步任意文件夹

2017.05.11 13:58* 字数 538 阅读 1545评论 0喜欢 4

## 前言

OneDrive 作为微软的云盘服务，已经集成到了 win10 系统中去，结合 OneNote、Office Online 等微软家服务使用起来倒也不错。但是并不能通过客户端**自由同步本地任意位置的文件夹**，想要实现这一功能可以通过链接的方式，下面就介绍一下具体的方法。

## 方法

场景：我需要在在 OneDrive 中创建一个“备份同步”文件夹来同步本地的“OneNote”备份文件。

#### 第一步

打开OneDrive文件夹，复制路径：
D:\OneDrive

#### 第二步

打开需要添加同步的本地文件夹，复制路径：
D:\Personal\OneNote备份

#### 第三步

以管理员权限运行 cmd，“备份同步”为要创建的文件夹：

```
mklink /d D:\OneDrive\备份同步 D:\Personal\OneNote备份
```

回车后显示：“为 D:\OneDrive\备份同步 <<===>> D:\Personal\OneNote备份 创建的符号链接”，即完成。

![img](https://upload-images.jianshu.io/upload_images/3334975-829fd5f7fb84e277.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/611)

Paste_Image.png

**注意：**OneDrive同步文件夹中若已存在此文件夹，则不能创建链接。

接下来就可以在本地文件夹增删文件，OneDrive中会自动同步。

## 结语

虽然 OneDrive 在国内的速度可能不是很理想，而且现在免费用户只有 5G 容量，比起动则上 T 容量的国内网盘来说有点“寒酸”，但是如果用来同步备份一些文档资料的话，还是可以的。

OneDrive 和 Windows 系统以及 Office Oneline 等的集成也带来了很大的便利，Office Oneline 现在的功能也十分完善，满足我们工作、学习文档的编写处理不在话下，只要有浏览器，我们就能登录 OneDrive 随时随地编辑文档。