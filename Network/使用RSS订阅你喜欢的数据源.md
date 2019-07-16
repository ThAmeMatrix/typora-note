# 使用 RSS 订阅你喜欢的频道

**RSS**（**简易信息聚合**）是一种消息来源格式规范，用以**聚合经常发布更新数据的网站**，例如博客文章、新闻、音频或视频的网摘。Really Simple Syndication “简易信息聚合”就是RSS的英文原意。把新闻标题、摘要（Feed）、内容按照用户的要求，“送”到用户的桌面就是RSS的目的。

比较热门的聚合类 App： 即刻、今日头条、Google 新闻



## 安装

**Github地址：**<https://github.com/DIYgod/RSSHub>，有问题或者想要其它网站`RSS`功能的，可去`Github`留言。
**项目地址：**<https://rsshub.js.org/>

**环境要求：**`Redis`、`Node.js`，这里只讲`CentOS`系统安装方法。

**1、安装EPEL**

```
#CentOS/RHEL 6：
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
#CentOS/RHEL 7：
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

**2、安装Redis**

```
#安装Redis
yum install redis
#启动 redis 服务
systemctl start redis
#返回结果PONG，则安装成功。
redis-cli ping
```

**3、安装Node.js**

```
curl -sL https://rpm.nodesource.com/setup_8.x | bash -
yum install nodejs -y
```

**4、安装RSSHub**

```
yum -y install git
git clone https://github.com/DIYgod/RSSHub.git
cd RSSHub && npm install
```

**5、运行RSSHub**
这里使用`Screen`运行程序。防止程序中断。

```
yum -y install screen
screen -S RSSHub
cd RSSHub
node index.js
```

然后使用`IP:1200`就可以访问了，如果需要使用域名并添加`SSL`的，接着看。



# CentOS Docker 安装

Docker支持以下的CentOS版本：

- CentOS 7 (64-bit)
- CentOS 6.5 (64-bit) 或更高的版本

------

## 前提条件

目前，CentOS 仅发行版本中的内核支持 Docker。

Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。

Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，要求系统为64位、系统内核版本为 2.6.32-431 或者更高版本。

------

## 使用 yum 安装（CentOS 7下）

Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。

通过 

uname -r 

命令查看你当前的内核版本

```
[root@runoob ~]# uname -r 3.10.0-327.el7.x86_64
```

![img](http://www.runoob.com/wp-content/uploads/2016/05/docker08.png)

### 安装 Docker

从 2017 年 3 月开始 docker 在原来的基础上分为两个分支版本: Docker CE 和 Docker EE。

Docker CE 即社区免费版，Docker EE 即企业版，强调安全，但需付费使用。

本文介绍 Docker CE 的安装使用。

移除旧的版本：

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

安装一些必要的系统工具：

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum install docker-compose
```

添加软件源信息：

```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

更新 yum 缓存：

```
sudo yum makecache fast
```

安装 Docker-ce：

```
sudo yum -y install docker-ce
```

启动 Docker 后台服务

```
sudo systemctl start docker
```

测试运行 hello-world

```
[root@runoob ~]# docker run hello-world
```

![img](http://www.runoob.com/wp-content/uploads/2016/05/docker12.png)

由于本地没有hello-world这个镜像，所以会下载一个hello-world的镜像，并在容器内运行。



## 使用 Docker 部署

运行下面的命令下载 RSSHub 镜像.

```bash
$ docker pull diygod/rsshub
```

然后运行 RSSHub 即可

```bash
$ docker run -d --name rsshub -p 1200:1200 diygod/rsshub
```

在浏览器中打开 <http://127.0.0.1:1200/>, enjoy it! ✅

您可以使用下面的命令来关闭 RSSHub.

```bash
$ docker stop rsshub
```

### 添加配置

配置运行在 docker 中的 RSSHub, 最便利的方法是使用 docker 环境变量.

以设置缓存时间为 1 小时举例, 只需要在运行时增加参数: `-e CACHE_EXPIRE=3600`

```bash
$ docker run -d --name rsshub -p 1200:1200 -e CACHE_EXPIRE=3600 -e GITHUB_ACCESS_TOKEN=example diygod/rsshub
```

更多配置项请看 [应用配置](https://docs.rsshub.app/install/#%E5%BA%94%E7%94%A8%E9%85%8D%E7%BD%AE)

### 使用 docker-compose 部署

1. 创建 volume 持久化 Redis 缓存

```bash
$ docker volume create redis-data
```

1. 修改 [docker-compose.yml](https://github.com/DIYgod/RSSHub/blob/master/docker-compose.yml) 中的 `environment` 进行配置
   - `PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=1` 用以跳过 puppeteer Chromium 的安装. 默认为 1, 需要在 `config.js` 中的 `puppeteerWSEndpoint`中设置相应的远程 Chrome Websocket 地址, 以启用相应路由.
   - `USE_CHINA_NPM_REGISTRY=1` 防止 npm 受到来自 GFW 的干扰. 默认为 0.
2. 部署

```bash
$ docker-compose up
```

1. 更新

```bash
$ docker-compose build
$ docker-compose up
```



# 删除本地 Docker 镜像

 [2016-09-28](http://csbun.github.io/blog/2016/09/docker-remove-image/)

使用 Docker 会遗留一大堆不知道什么鬼的镜像，下面我们想办法干掉他们。

## TL;DR

```
docker ps -a | grep 'Exited' | awk '{print $1}' | xargs docker stop | xargs docker rm
docker images | grep '<none>' | awk '{print $3}' | xargs docker rmi
```



## 直接删除

启动 Docker（如果没有启动的话），mac 下运行如下命令：

```
boot2docker start
```



然后可以查看现有的本地镜像

```
docker images
```

从如下的输出结果可以看到有很多没有但占据硬盘空间的镜像：

```
REPOSITORY                         TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
csbun/docker-centos6.6-java7-git   1.0                 4daba39c348a        3 weeks ago         1.183 GB
csbun/docker-centos6.6-java7-git   latest              4daba39c348a        3 weeks ago         1.183 GB
<none>                             <none>              e146cf64f30e        3 weeks ago         1.181 GB
<none>                             <none>              139e0f6180fb        4 weeks ago         534 MB
...
centos                             6.6                 8668efb40032        12 weeks ago        202.6 MB
hello-world                        latest              91c95931e552        17 months ago       910 B
```

于是我们可以直接通过上面的 `IMAGE ID` 删除这个镜像

```
docker rmi <image-id>
```

如无意外，这个镜像嘟嘟嘟就会被干掉。然而很不幸大多数时候都会提示下来错误，表示镜像正在被某个容器 `container` 使用：

```
Error response from daemon: Conflict, cannot delete 91c95931e552 because the container 429166761c13 is using it, use -f to force
```

如上，`container id` 为 `429166761c13`，知道这个 id 我们就能停止容器，然后重新删除镜像：

```
docker ps -a | grep <container-id>
docker stop <container-id>
docker rm <container-id>
docker rmi <image-id>
```

## 批量删除

但是镜像好多怎么办？容器更多怎么办？只能写脚本批量删除了：

首先我们通过 `docker ps -a` 找出所有已经退出的容器并用 `docker stop` 和 `docker rm` 将其干掉，保证镜像没有被占用：

```
docker ps -a | grep 'Exited' | awk '{print $1}' | xargs docker stop | xargs docker rm
```

然后再尝试删除镜像：

```
docker images | grep '<none>' | awk '{print $3}' | xargs docker rmi
```

OK，一切都干净了。