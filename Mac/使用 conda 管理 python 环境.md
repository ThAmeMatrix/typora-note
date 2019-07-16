# 使用conda管理python环境

来源：<https://zhuanlan.zhihu.com/p/22678445>

量化投资，R/Python，旅行

113 人赞同了该文章

## 一、动机

最近打算折腾[vn.py](https://link.zhihu.com/?target=https%3A//github.com/vnpy/vnpy)，但只有py27版本的，因为一向习惯使用最新稳定版的，所以不得不装py27的环境，不得不说 
Python的全局锁真的很烦。 
身为懒癌患者，必然使用全功能的anaconda，但不想同时装py27和py35两个版本的anaconda巨无霸（同时装两个， 
不知道conda是否也可以管理环境），于是选择用conda装python27的环境及一些必要的包。 
弄了几天终于把办公电脑和家里的Mac机上的环境都配好了，即使有了官方的安装教材，也踩了不少坑。 
(因为国内主要的期货交易API接口只有windows和linux版，所以Mac上的环境只能用来回测，无法使用vn.py的模拟交易和实盘功能。)

## 二、环境管理

### conda常用命令

- 查看当前系统下的环境

```text
conda info -e
```

- 创建新的环境

```text
# 指定python版本为2.7，注意至少需要指定python版本或者要安装的包# 后一种情况下，自动安装最新python版本
conda create -n env_name python=2.7
# 同时安装必要的包
conda create -n env_name numpy matplotlib python=2.7
```

- 环境切换

```text
# 切换到新环境# linux/Mac下需要使用source activate env_name
activate env_name
#退出环境，也可以使用`activate root`切回root环境
deactivate env_name
```

- 移除环境

```text
conda remove -n env_name --all
```

## 三、包管理

- 给某个特定环境安装package有两个选择，一是切换到该环境下直接安装，二是安装时指定环境参数-n

```text
activate env_nameconda install pandas
# 安装anaconda发行版中所有的包
conda install anaconda
conda install -n env_name pandas
```

- 查看已经安装的package

```text
conda list
# 指定查看某环境下安装的package
conda list -n env_name
```

- 查找包

```text
conda search pyqtgraph
```

- 更新包

```text
conda update numpy
conda update anaconda
```

- 卸载包

```text
conda remove numpy
```

## 四、vnpy环境配置中遇到的疑难杂症

### 1、64位系统和root环境下指定安装32位

vnpy在window系统下使用的python版本和package都是32位的，但除非下载anaconda时就下载32位版本， 
现在大多数系统都是64位了吧，我装的也是64位，那么用conda安装时默认64位，stackoverflow了发现解 
决方案，安装前设置使用32位：

```text
# 设置32位set CONDA_FORCE_32BIT=1
conda create -n env_name python=2.7
conda install numpy pandas
# 切回系统默认set CONDA_FORCE_32BIT=
```

### 2、设置国内镜像

家里用的长城宽带，访问国外资源的网速简直不能忍，于是看了下conda有没有国内的镜像。然后真找到了一个 
清华大学TUNA镜像[清华大学 TUNA 镜像源](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/help/anaconda/)
网站有添加方法

```text
# 需要去掉网址的引号
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/conda config --set show_channel_urls yes
```

如果命令行方法添加不上，可以在用户目录下的**.condarc**中添加*https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/*： 
如果没有该文件可以直接创建，Windows为**C://Users/username/.condarc**，Linux/Mac为**~/.condarc**
结果如下：

```text
channels:
 - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ 
 - defaults
show_channel_urls: yes
```