# Linux 使用 conda 管理多个 python 环境

```shell
// Anaconda创建环境：
//下面是创建python=3.6版本的环境，取名叫py36
conda create -n py36 python=3.6 

// 删除环境
conda remove -n py36 --all

// 激活环境
// 下面这个py36是个环境名
source activate py36

// 退出环境
source deactivate

// 查看安装了哪些包
conda list

// 查看当前存在哪些虚拟环境
conda env list & conda info -e 

// 检查更新当前conda
conda update conda 

// 更新本地已安装的包
conda update --all 
```



python模块安装，使用国内源可以提高下载速度。

pip源更改：
pip源有好几个，我一直用的清华的pip源，它5分钟同步一次。 
临时使用： 
pip 后加参数 -i https://pypi.tuna.tsinghua.edu.cn/simple 
例：pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas 
永久使用： 
Linux下： 
修改 ~/.pip/pip.conf (没有就创建一个)， 修改 index-url至tuna，内容如下：

    [global]
    index-url = https://pypi.tuna.tsinghua.edu.cn/simple

windows下: 
直接在user目录中创建一个pip目录，如：C:\Users\xxxx\pip，新建文件pip.ini，内容如下

 [global]
 index-url = https://pypi.tuna.tsinghua.edu.cn/simple

conda源更改：
conda源国内只有清华有， 
修改源只需输入如下两行命令：

    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --set show_channel_urls yes
---------------------
作者：DivinerShi 
来源：CSDN 
原文：https://blog.csdn.net/sxf1061926959/article/details/54091748 



## ImageAI 安装：

### **Dependencies**

To use 

ImageAI

 in your application developments, you must have installed the following dependencies before you install 

ImageAI :



**- Python 3.5.1 (and later versions)** [Download](https://www.python.org/downloads/) (Support for Python 2.7 coming soon) 
**- pip3** [Install](https://pypi.python.org/pypi/pip) 
**- Tensorflow 1.4.0 (and later versions)** [Install](https://www.tensorflow.org/install/install_windows) or install via pip

```
 pip3 install --upgrade tensorflow 
```

**- Numpy 1.13.1 (and later versions)** [Install](https://www.scipy.org/install.html)

 or install via pip

```
 pip3 install numpy 
```

**- SciPy 0.19.1 (and later versions)** [Install](https://www.scipy.org/install.html)

 or install via pip

```
 pip3 install scipy 
```

**- OpenCV** [Install](https://pypi.python.org/pypi/opencv-python)

 or install via pip

```
 pip3 install opencv-python 
```

**- Pillow** [Install](https://pypi.org/project/Pillow/2.2.1/)

 or install via pip

```
 pip3 install pillow 
```

**- Matplotlib** [Install](https://matplotlib.org/users/installing.html)

 or install via pip

```
 pip3 install matplotlib 
```

**- h5py** [Install](http://docs.h5py.org/en/latest/build.html)

 or install via pip

```
 pip3 install h5py 
```

**- Keras 2.x** [Install](https://keras.io/#installation)

 or install via pip

```
 pip3 install keras 
```





### **Installation**

To install ImageAI, run the python installation instruction below in the command line: 

**pip3 install https://github.com/OlafenwaMoses/ImageAI/releases/download/2.0.2/imageai-2.0.2-py3-none-any.whl** 



or download the Python Wheel [**imageai-2.0.2-py3-none-any.whl**](https://github.com/OlafenwaMoses/ImageAI/releases/download/2.0.2/imageai-2.0.2-py3-none-any.whl) and run the python installation instruction in the command line to the path of the file like the one below: 

**pip3 install C:\User\MyUser\Downloads\imageai-2.0.2-py3-none-any.whl** 