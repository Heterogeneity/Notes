# 创建环境
jupyter notebook不会自动检测新环境，默认会使用anaconda根目录下的python环境（base）。使用新环境，首先要激活环境，在其中下载jupyter或ipykernel，有两种方式：

1. anaconda navigator图形界面中的environments中选中要进入的环境，激活后回到Home主页安装jupyter notebook启动，然后打开jupyter即可在新建选项中选择内核。

2. 在anaconda prompt命令行界面
```
conda activate your_env_name 激活环境
pip install jupyter -i 任意源  完成jupyter安装。
conda activate your_env_name 激活环境
conda install ipykernel
```
到这一步如果jupyter安装了`nb_conda`插件，jupyter会自动检测该环境内核，在jupyter notebook中内核名称为`Python [conda env:环境名]`，如果没有，继续如下操作：

`ipython kernel install --user --name= "name" –display-name= "display-name"  将内核注册到jupyter`

或`python -m ipykernel install --user --name=my_custom_kernel --display-name="My Custom Kernel"  用这条命令也可以`

name指内核注册的名字，display-name指该内核在jupyter notebook中显示的名字。

至此，环境创建完成。附上[ipython官方文档教程](https://ipython.readthedocs.io/en/stable/install/kernel_install.html)，其中有详细的创建多环境命令教程。

下一步打开jupyter notebook（如果该环境没有安装jupyter，那么启动的是base的jupyter）即可`new`不同环境进行编程。

**以上两种方式建议只安装或注入内核kernel即可（实际上安装jupyter的本质也是安装kernel内核）。**

---

# 查看已注册内核与内核规范kernelspec

`jupyter kernelspec list`命令可查看注册在jupyter notebook中的内核。

## 内核规范kernelspec(kernel specification)

> 内核规范（Kernel Specification）是一组元数据文件，定义了 Jupyter 内核的配置信息。每个内核都有一个关联的内核规范，其中包含描述内核的元数据，例如内核的名称、执行程序的路径、图标等信息。
> 
> 通常，Jupyter 内核规范被存储在以下位置：
> 
> **Linux 和 macOS： ~/.local/share/jupyter/kernels/**
> 
> **Windows：%APPDATA%\jupyter\kernels\\**

这里%APPDATA%为C:\Users\用户名\AppData\Roaming(使用`echo`命令查看)。

我的ipykernel安装在conda环境里，我这里是D:\anaconda3\envs\exp-1\。

之前提到的`Python [conda env:环境名]`指的是，安装了nb_conda的jupyter notebook会自动去conda环境里找kernel，这样我们就可以在notebook中看到我们环境的内核。nb_conda具体功能和用法参见[nb_conda的GitHub仓库地址](https://github.com/Anaconda-Platform/nb_conda)。

但是`jupyter kernelspec list`查看不到nb_conda识别出来的内核，因为它们不在%APPDATA%\jupyter\kernels中。而当我们使用`ipython kernel install --user --name= "name" –display-name= "display-name"`后，

在D盘的内核被注册到C盘下的内核规范文件夹中，这个内核规范kernel.json文件会记录内核的路径（D盘），--name 即是%APPDATA%\jupyter\kernels下kernel的名称。

假设我们先前设置kernel的名称为`kernel1`此时，jupyter notebook会出现两个内核选项：

`Python [conda env:kernel1]`和`kernel1`

它们两个都指向了同一个环境的内核，区别在于一个是由nb_conda检测出来的，另一个是注册在jupyter notebook中的，实际上是两个不同路径下的kernel.json文件指向了同一个python解释器，使用起来并无差别。

---

# nb_conda安装命令

nb_conda可以在jupyter notebook文件中实现实时切换不同的环境内核，在anaconda prompt命令行执行

`conda install -c conda-forge nb_conda`即可安装，安装成功后jupyter页面出现conda选项卡（base）：
 ![image](https://github.com/Heterogeneity/Notes/assets/102458836/8bb243c7-5cb4-4834-8f7f-064a844ff5c5)
