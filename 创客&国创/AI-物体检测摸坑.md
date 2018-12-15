# AI-物体检测摸坑



## 环境配置

> 参考自 Darknet 官网: https://pjreddie.com/darknet/yolo/

环境要求:

```shell
ubuntu16.04
python3.6.3(anaconda)
opencv-python3
CUDA+CUDNN
yolov3 darknet
```

首先, Darknet 的安装:

```shell
git clone https://github.com/pjreddie/darknet
cd darknet
make
```

下载yolov3 预训练权重:

```shell
wget https://pjreddie.com/media/files/yolov3.weights
```



## 网络摄像头上的实时检测

如果您看不到结果，则在测试数据上运行YOLO并不是很有趣。而不是在一堆图像上运行它让我们在网络摄像头的输入上运行它！

要运行此演示，您需要[使用CUDA和OpenCV](https://pjreddie.com/darknet/install/#cuda)编译[Darknet](https://pjreddie.com/darknet/install/#cuda)。

### 1. OpenCV

Opencv可以直接从库中安装，也可以自己手动编译安装, **从库中安装是最简单的方式，直接运行下面命令：**

```shell
sudo apt-get install libopencv-dev python-opencv
```

如果安装出错，那么可以更新一下源，或是换一个源。



### 2. CUDA, Cudnn

参考: https://blog.csdn.net/u014595019/article/details/53732015

### 3. 编译 Darknet

当 OpenCV 和 CUDA 安装完成后, 将基本目录中的`Makefile`的前三行更改为：

```makefile
GPU=1
CUDNN=1
OPENCV=1
```

然后, 重新构建项目:

```shell
make
```

### 4. 测试

配置好 OpenCV 和 CUDA 后, 运行命令:

```shell
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights
```

2018.7.13 : darknet 预训练模型 real - time webcam objectdetection 测试摄像头实时检测demo:

![2018-07-13 20-18-26屏幕截图](/home/nicholascheng/Pictures/PrintScreen/2018-07-13 20-18-26屏幕截图.png)



## 训练

### 1. 收集数据集

**Image-Downloader (批量爬取主流搜索引擎图片).**  在 Linux 环境下运行.

```shell
git clone https://github.com/sczhengyabin/Image-Downloader.git
```

#### 1.1 安装依赖库

```shell
apt-get install python3-pip python3-pyqt5 pyqt5-dev-tools
pip3 install -r requirements.txt	//安装 python 依赖
```

#### 1.2 安装 phantomjs

0. 介绍

> PhantomJS是一个基于webkit的JavaScript API。它使用QtWebKit作为它核心浏览器的功能，使用webkit来编译解释执行JavaScript代码。任何你可以在基于webkit浏览器 做的事情，它都能做到。它不仅是个隐形的浏览器，提供了诸如CSS选择器、支持Web标准、DOM操作、JSON、HTML5、Canvas、SVG等， 同时也提供了处理文件I/O的操作，从而使你可以向操作系统读写文件等。PhantomJS的用处可谓非常广泛，诸如前端无界面自动化测试（需要结合 Jasmin）、网络监测、网页截屏等。

1. 下载

从官网 <http://phantomjs.org/download.html> 下载 phantomjs。

2. 解压, 添加到环境变量

```shell
tar -xvjf phantomjs-2.1.1-linux-x86_64.tar.bz2  
sudo cp -R phantomjs-2.1.1-linux-x86_64 /usr/local/share/  
sudo ln -sf /usr/local/share/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/local/bin/
```

3. 测试是否安装成功  

```shell
phantomjs --version  
```

还可以直接在命令行界面中输入phantomjs进入到phantomjs的环境

#### 1.3 运行 Image-Downloader

```shell
git clone https://github.com/sczhengyabin/Image-Downloader.git
cd Image-Downloader
python image_downloader_gui.py
```

运行的 GUI 界面:

![选区_208](/home/nicholascheng/Pictures/ScreenShots/选区_208.png)



### 2. 打标签

#### 2.1 使用工具: BBox-Label-Tool

#### Environment

- python 2.7
- python PIL (Pillow)

#### 转换图片格式: 

```shell
mogrify -format JPEG *.jpeg
mogrify -format JPEG *.png
rm *.png
rm *.jpeg
```

#### 运行:

```shell
source activate py2	  //激活python2 环境
python main.py
```



#### 2.2 使用工具: labelImg

项目地址: https://github.com/tzutalin/labelImg

#### 安装

#### Download prebuilt binaries

- [Windows & Linux](https://tzutalin.github.io/labelImg/)

在下载的文件中, 运行报错:

```shell
sudo ./labelImg 
[sudo] nicholascheng 的密码： 
Cannot mix incompatible Qt library (version 0x40807) with this library (version 0x40806)
已放弃 (核心已转储)
```

无法在 ubuntu 16.04 上运行, 目前作者标记为一个 bug, issues 窗口:https://github.com/tzutalin/labelImg/issues/61

对此, 改用 Docker 安装:

#### Use Docker

```shell
// 注意, docker 推荐在root权限下运行.

// 拉取并运行镜像:
docker run -it \
--user $(id -u) \
-e DISPLAY=unix$DISPLAY \
--workdir=$(pwd) \
--volume="/home/$USER:/home/$USER" \
--volume="/etc/group:/etc/group:ro" \
--volume="/etc/passwd:/etc/passwd:ro" \
--volume="/etc/shadow:/etc/shadow:ro" \
--volume="/etc/sudoers.d:/etc/sudoers.d:ro" \
-v /tmp/.X11-unix:/tmp/.X11-unix \
tzutalin/py2qt4

make qt4py2
./labelImg.pydocker run
```

#### docker 简单使用:

参考: https://letong.gitbooks.io/docker/content/images/rmi.html

查看镜像:

```shell
sudo su	// 切换到root用户
docker images
```

删除镜像:

```shell
docker rmi <imageID>
```

如遇无法删除，请删除依赖相关容器

```shell
docker rm `docker ps -aq`
```
然而, 使用 docker 运行时, 仍会出现 No Moudule named "PyQt4"的错误, 最后放弃, 使用该工具的 Windows 二进制版本.



####  ps: 在使用 git clone 期间经常出现错误:

```shell
RPC failed; curl 56 GnuTLS recv error (-9): A TLS packet with unexpected length was received.
```

 于是重新安装 git:

```shell
sudo apt purge git
sudo apt install git
```


#### 开始打标

这一步没什么好说的, 强行打...



### 准备训练

**下载预训练卷积权重：**

对于训练，我们使用在Imagenet上预训练的卷积权重。我们使用[darknet53](https://pjreddie.com/darknet/imagenet/#darknet53)模型的权重。您可以在[此处](https://pjreddie.com/media/files/darknet53.conv.74)下载卷积图层的权重[（76 MB）](https://pjreddie.com/media/files/darknet53.conv.74)。

### 准备YOLOv3配置文件

YOLOv3需要某些特定文件来了解如何以及如何训练。我们将创建这三个文件（.data，.names和.cfg），并解释yolov3.cfg和yolov3-tiny.cfg。

- data/ssd-test-obj.data
- cfg/ssd-test-obj.names

首先让我们准备YOLOv3  *.data*和  *.names*文件。让我们首先创建 ssd-test-obj.data 并用这个内容填充它。这基本上说我们正在训练一个类，训练和验证集文件是什么，以及哪个文件包含我们想要检测的类别的名称。 backup 指定了存放训练权重的目录.

```c
classes = 1
train = ssd-train.txt
valid = ssd-test.txt
names = data/ssd-test-obj.names
backup = backup/
```

该 ssd-test-obj.names*看起来是这样的，简单明了。每个新类别都应该在新行上，其行号应与我们之前创建的*.txt标签文件中的类别编号相匹配  。

```
ssd1306
```



创建新目录 train-config, 用于存放训练的配置文件

```shell
mkdir train-config	//该目录下放训练的配置文件
// ssd-train.txt, ssd-test.txt
```



使用 python 脚本: process.py 生成 对应的 train 和 test 的 txt 文件

训练:

```shell
./darknet detector train cfg/ssd-test-obj.data cfg/ssd-test-yolov3-tiny.cfg darknet53.conv.74
```

```shell
./darknet detector train cfg/ssd-test-obj.data cfg/ssd-test-yolov3.cfg darknet53.conv.74
```

```
./darknet detector train cfg/nodemcu-test-obj.data cfg/ssd-test-yolov3.cfg darknet53.conv.74
```



遇到的错误: 训练出来的结果都是 NAN, 暂时没有解决方案

![选区_225](/home/nicholascheng/Pictures/ScreenShots/选区_225.png)





![选区_242](/home/nicholascheng/Pictures/ScreenShots/选区_242.png)

测试:

```
./darknet detector test cfg/ssd-test-obj.data cfg/ssd-test-yolov3.cfg backup/ssd-test-yolov3_final.weights data/Bing_0021.png
```

