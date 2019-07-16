# 安装Manjaro后该做的事

## 更改源

自动选择中国源：

```
sudo pacman-mirrors -gb testing -c China    //选择中国源并更新
sudo pacman -Syyu                           //更新系统
```

## 安装软件时，提示PGP签名损坏

需要安装archlinuxcn-keyring

```
sudo pacman -S archlinuxcn-keyring
```

## 修改Home下的目录为英文

1. 修改目录映射文件名；

```
vim .config/user-dirs.dirs
```

修改为一下内容：

```
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Download"
XDG_TEMPLATES_DIR="$HOME/Templates"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Documents"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Pictures"
XDG_VIDEOS_DIR="$HOME/Videos"
```

1. 将Home目录下的中文目录名改为对应的中文名；
2. 重启系统。

## 软件安装

### Chrome

```
sudo pacman -S google-chrome
```

### 搜狗输入法

1. 安装fcitx

```
sudo pacman -S fcitx
```

> 安装时会提示安装那些不见，直接按回车，安装全部。

1. 安装搜狗输入法

```
sudo pacman -S fcitx-sogoupinyin
```

1. 配置，在`~/.xprifile中添加

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

### zsh & oh-my-zsh:

1. 安装zsh:

```
sudo pacman -S zsh
```

1. 使用zsh替换bash:

```
chsh -s /bin/zsh
```

> 需要重启系统生效.

1. 安装oh-my-zsh

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

1. 安装autojump

```
sudo pacman -S autojump
```

再在`~/.zshrc`中添加:

```
plugins=(git autojump)
```

### vim & spf13

1. 安装vim

```
sudo pacman -S vim
```

1. 安装spf13-vim配置

```
curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
```

### JDK

```
sudo pacman -S jdk8
```

配置环境:

```
export JAVA_HOME=/usr/lib/jvm/default
export JRE_HOME=${JAVA_HOEM}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib 
```

### 虚拟机

Archlinux维基有更详细的安装说明:[VMware(简体中文)](https://link.jianshu.com/?t=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%2FVMware_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29).

VMware workstation 14 激活密钥:

```
CG54H-D8D0H-H8DHY-C6X7X-N2KG6

ZC3WK-AFXEK-488JP-A7MQX-XL8YF

AC5XK-0ZD4H-088HP-9NQZV-ZG2R4

ZC5XK-A6E0M-080XQ-04ZZG-YF08D

ZY5H0-D3Y8K-M89EZ-AYPEG-MYUA8

FF590-2DX83-M81LZ-XDM7E-MKUT4

FF31K-AHZD1-H8ETZ-8WWEZ-WUUVA

CV7T2-6WY5Q-48EWP-ZXY7X-QGUWD

AALYG-20HVE-WHQ13-67MUP-XVMF3
```

### uget & aria2

```
sudo pacman -S uget
sudo pacman -S aria2
```

然后在uget的界面点击: 编辑->设置->插件, 在插件匹配顺序选择aria2

### 微信

```
sudo pacman -S electronic-wechat
```

### 坚果云

```
sudo pacman -S nutstore
```

### shadownsocks-qt5

```
sudo pacman -S shadownsocks-qt5
```



# Manjaro 安装后配置

##### 1. 将本地数据包与远程数据包同步

```
sudo pacman -Syy
```

默认manjaro是没有同步数据包的，也就是说，这个时候你执行`pacman -S pack_name` 会报数据包找不到的错误(`warning: database file for 'core' does not exist` ...)

##### 2. 安装vim

vim 无疑是linux下最好用的编辑器之一，为了方便我们待会修改配置文件，可以先将这个软件装上

```
sudo pacman -S vim
```

**如果我们没有执行第一步操作，这个时候直接安装是会报错的**

##### 3. 添加archlinxCN源

用vim编辑`/etc/pacman.conf`文件(`sudo vim /etc/pacman.conf`)，在文件底部添加下面几行:

```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server =https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

修改配置文件后，执行下面的命令：

```
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```

##### 4. 安装ZSH

ZSH可以说是shell中的极品，它的优点太多了，我就不一一写出来，有兴趣的同学可以看这篇文章[知乎-为什么说zsh是shell中的极品](https://link.jianshu.com?t=https%3A%2F%2Fwww.zhihu.com%2Fquestion%2F21418449)

我们首先将git安装好，因为我们带回配置的时候需要用到git

```
sudo pacman -S git
```

接着安装zsh

```
sudo pacman -S zsh
```

然后配置oh-my-zsh

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

更换默认的shell

```
chsh -s /bin/zsha
```

好了，zsh已经安装完了

##### 5. 安装搜狗输入法

我们安装基于fcitx的搜狗输入法

```
sudo pacman -S fcitx-sogoupinyin
sudo pacman -S fcitx-im         # 全部安装
sudo pacman -S fcitx-configtool # 图形化配置工具
```

设置中文输入法环境变量，编辑~/.xprofile文件，增加下面几行(如果文件不存在，则新建)

```
exportGTK_IM_MODULE=fcitx
exportQT_IM_MODULE=fcitx
exportXMODIFIERS="@im=fcitx"
```

保存成功后，在终端输入fcitx启动服务后，会增加一个键盘的托盘图标，右击这个图标，打开配置工具，在输入法栏目中增加sogoupinyin输入法。

重启后就能正常使用了。

##### 6. 常用软件安装

- 谷歌浏览器 (sudo pacman -S chromium)
- 网易云音乐 (sudo pacman -S netease-cloud-music)

其他的软件就不多说了，大家可以自行去[AUR](https://link.jianshu.com?t=https%3A%2F%2Faur.archlinux.org%2F)上查找.

作者：单板小智

链接：https://www.jianshu.com/p/79dae972b1e9

來源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。



2018最新Manjaro安装配置教程。别看网上那些乱七八糟的老掉牙教程了，大部分都不太对。
 注意，本教程的时效：作者版本Manjaro 18.0.0-rc Illyria，时间过久之后请读者注意本文的有效性。
 以下部分按照步骤先后。

//配置中国镜像源：
 pacman：sudo pacman-mirrors -i -c China -m rank然后手动选择清华镜像
 检查镜像源：gedit /etc/pacman-mirrors.conf
 //更新系统：
 sudo pacman -Syu
 //安装AUR配置AUR中国镜像：
 ~~sudo pacman -S yaourt~~
 （yaourt已经停止开发，被社区标记为不推荐）
 sudo pacman -S aurman
 ~~在/etc/yaourtrc修改AURURL="https://aur.tuna.tsinghua.edu.cn"~~
 在“添加/删除软件”里启用aur镜像。
 //配置显卡驱动（开源）：
 使用manjaro settings manager
 //安装输入法：
 sudo pacman -S fcitx fcitx-im fcitx-cloudpinyin
 //配置输入法环境变量：
 sudo gedit /etc/environment
 加入
 export GTK_IM_MODULE=fcitx
 export QT_IM_MODULE=fcitx
 export XMODIFIERS=@im=fcitx
 注销登录
 输入法选项完善
 //设置ssr：
 安装libappindicator-gtk2（为了托盘显示，否则程序启动后找不到GUI入口）
 添加/删除程序搜索electron-ssr编译安装
 打开设置ssr
 //登录chrome，搜索安装gnome插件，然后弄里面的插件
 //添加/删除程序完全卸载系统不必要程序（根据条目判断）
 //美化：
 ~/.local/share创建themes，fonts，icons三个文件夹，把那些主题包扔进去
 GTK和shell主题放在themes，鼠标样式和图标放在icons
 //安装程序：
 deepin.com.im.qq
 copyq
 sdkman
 gitkraken
 deepin取色器
 wps-office
 ttf-wps-fonts
 thunderbird
 Filezilla
 GIMP
 cloud-music
 intellij-idea
 android-studio
 pycharm
 remember the milk
 visual-studio-code
 haroopad
 wechat
 deepin截图
 vmware-workstation
 （其他）
 //可能存在的问题：
 //开机grub引导5s，太慢了。
 按网上的方法直接编辑grub.cfg是不行的。gedit /boot/grub/grub.cfg，查看会发现文件里有注释It is automatically generated by grub-mkconfig using templates from /etc/grub.d and settings from /etc/default/grub，所以我们修改/etc/default/grub文件中将GRUB_TIMEOUT=5改成=0。0没有关系，因为那个引导过程大部分时间没有用
 ，系统修复也不需要。不过前提是注意使用TimeShift及时备份重要更改以免翻船。

作者：悬雨

链接：https://www.jianshu.com/p/27fe99f3ee13

來源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。